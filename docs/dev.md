
## Developer Documentation 

The following table includes links to Cloudbreak developer documentation: 

| Doc Link | Description |
|---|---|
| [Set Up Local Development](https://github.com/hortonworks/cloudbreak) | This documentation will help you set up your local development environment. | 
| [Retrieve OAuth Bearer Token via Cloudbreak REST API](#retrieve-oauth-bearer-token-via-cloudbreak-rest-api) | Describes how to retrieve OAuth bearer token via Cloudbreak REST API. |
| [SPI Reference](#cloudbreak-service-provider-interface-spi) | This is Cloudbreak SPI reference documentation. |
| [API Reference](https://app.swaggerhub.com/apis/Cloudbreak/Cloudbreak/2.4.1) | This is Cloudbreak API reference documentation. Cloudbreak is a RESTful application development platform whose goal is to help developers deploy HDP clusters in various cloud environments. Once Cloudbreak is deployed in your favorite servlet container, it exposes REST APIs, allowing you to spin up Hadoop clusters of any size with your chosen cloud provider. |
| [Flow Diagrams](http://hortonworks.github.io/cloudbreak-docs/release-1.16.5/flow/) | This is Cloudbreak flow diagrams reference documentation. |

[Comment]: <> (API link should be updated for each release. Not sure about the flow diagrams?)


### Retrieve OAuth Bearer Token via Cloudbreak REST API

In order to communicate with Cloudbreak's API, you must retrieve a bearer token. For example: 

<pre>TOKEN=$(curl -k -iX POST -H "accept: application/x-www-form-urlencoded" -d 'credentials={"username":"admin@example.com","password":"pwd"}' "https://192.168.99.100/identity/oauth/authorize?response_type=token&client_id=cloudbreak_shell&scope.0=openid&source=login&redirect_uri=http://cloudbreak.shell" | grep location | cut -d'=' -f 3 | cut -d'&' -f 1)</pre>


### Cloudbreak Service Provider Interface (SPI)

In addition to supporting multiple cloud platforms, Cloudbreak provides an easy way to integrate a new provider trough its [Service Provider Interface (SPI)](https://github.com/hortonworks/cloudbreak/tree/master/cloud-api), a plugin mechanism that enables seamless integration with any cloud provider. 

This SPI plugin mechanism has been used to integrate all currently supported providers with Cloudbreak. The following links point to the Cloudbreak SPI implementations for AWS, Azure, Google Cloud, and OpenStack. You can use these implementations as a reference:
 
 * The [cloud-aws](https://github.com/hortonworks/cloudbreak/tree/master/cloud-aws) module integrates Amazon Web Services
 * The [cloud-azure](https://github.com/hortonworks/cloudbreak/tree/master/cloud-azure) module integrates Microsoft Azure
 * The [cloud-gcp](https://github.com/hortonworks/cloudbreak/tree/master/cloud-gcp) module integrates Google Cloud Platform  
 * The [cloud-openstack](https://github.com/hortonworks/cloudbreak/tree/master/cloud-openstack) module integrates OpenStack

The Cloubdreak SPI interface is event-based, scalable, and decoupled from Cloudbreak. The core of Cloudbreak uses [EventBus](http://projectreactor.io/) to communicate with the providers, but the complexity of event handling is hidden from the provider implementation.

#### Supported Resource Management Methods

Cloud providers support two kinds of deployment and resource management methods:

* Template based deployments
* Individual resource based deployments

Cloudbreak's SPI supports both of these methods. It provides a well-defined interface, abstract classes, and helper classes, scheduling and polling of resources to aid the integration and to avoid any boilerplate code in the module of cloud provider.

[comment]: <> (TO-DO: The sentence above is confusing. Does the clause "scheduling and polling of resources to aid the integration and to avoid any boilerplate code in the module of cloud provider" refer to the helper classes? Or should this say "It provides a well-defined interface, abstract classes, helper classes, **and** scheduling and polling of resources to aid the integration and to avoid any boilerplate code in the module of cloud provider.")

##### Template Based Deployments

Providers with template-based deployments such as [AWS CloudFormation](https://aws.amazon.com/cloudformation/), [Azure ARM](https://azure.microsoft.com/en-us/documentation/articles/resource-group-overview/#) or [OpenStack Heat](https://wiki.openstack.org/wiki/Heat) have the ability to create and manage a collection of related cloud resources, provisioning and updating them in an orderly and predictable fashion. 

When working with providers that use template-based deployments, Cloudbreak needs to be provided with a reference to the template, because every change in the infrastructure (for example, creating a new instance or deleting one) is managed through this templating mechanism.

If a provider has templating support, then the provider's [gradle](http://gradle.org/) module depends on the [cloud-api](https://github.com/hortonworks/cloudbreak/tree/master/cloud-api) module:

```
apply plugin: 'java'

sourceCompatibility = 1.7

repositories {
    mavenCentral()
}

jar {
    baseName = 'cloud-new-provider'
}

dependencies {

    compile project(':cloud-api')

}
```

The entry point for the provider is the  [CloudConnector](https://github.com/hortonworks/cloudbreak/blob/master/cloud-api/src/main/java/com/sequenceiq/cloudbreak/cloud/CloudConnector.java) interface and every interface that needs to be implemented is reachable trough this interface.

##### Individual Resource Based Deployments

There are providers such as GCP that do not support a templating mechanism, and customizable providers such as OpenStack where the Heat Orchestration (templating) component is optional and individual resources need to be handled separately. 

When working with such providers, resources such as networks, discs, and compute instances need to be created and managed with an ordered sequence of API calls, and Cloudbreak needs to provide a solution to manage the collection of related cloud resources as a whole.

If the provider has no templating support, then the provider's [gradle](http://gradle.org/) module typically depends on the [cloud-template](https://github.com/hortonworks/cloudbreak/tree/master/cloud-template) module, which includes Cloudbreak-defined abstract template. This template is a set of abstract and utility classes to support provisioning and updating related resources in an orderly and predictable manner trough ordered sequences of cloud API calls:

```
apply plugin: 'java'

sourceCompatibility = 1.7

repositories {
    mavenCentral()
}

jar {
    baseName = 'cloud-new-provider'
}

dependencies {

    compile project(':cloud-template')

}
```

#### Support for Modularity 

Cloudbreak uses **variants** to deal with highly modular providers such as OpenStack, which allows you to install different components (for volume storage, networking, and so on) and to entirely exclude certain components. For example, Nova or Neutron can be used for networking in OpenStack and some components such as Heat may not installed at all in some deployment scenarios. 

Cloudbreak SPI interface uses variants to support this flexibility: if some part of the cloud provider uses a different component, you don't need to re-implement the complete stack, but just use a different variant and re-implement the part that is different.

An example implementation for this feature can be found in the [cloud-openstack](https://github.com/hortonworks/cloudbreak/tree/master/cloud-openstack) module which supports a HEAT and NATIVE variants. While the HEAT variant utilizes the Heat templating to launch a stack, the NATIVE variant starts the cluster by using a sequence of API calls without Heat to achieve the same result. Both of them use the same authentication and credential management.
