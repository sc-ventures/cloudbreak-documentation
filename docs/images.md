## Custom Images

By default, Cloudbreak launches clusters from **standard default images**, which include default configuration and default tooling. These images are available by default for each supported provider and region. 

Cloudbreak includes both base images and prewarmed images, depending on the cloud provider. 

* **Base images** do not include Ambari or HDP software. These images can be used regardless of your choice of Ambari or HDP because that software is installed as part of the VM create and cluster provisioning process. This means you have more flexibility to use a base image but it can take longer to provision since more software is installed during cluster create.

* **Prewarmed images** include Ambari and HDP software pre-installed. This reduces the VM create and cluster provisioning process time but these images can only be used with a specific Ambari and HDP version.

| Cloud Provider | Base Images | Prewarmed Images |
|---|---|---|
| AWS | Amazon Linux 2017 | <ul><li>Amazon Linux 2017 with HDP 2.6 and Ambari 2.5</li><li>Amazon Linux 2017 with HDP 2.5 and Ambari 2.4</li></ul> |
| Azure | CentOS 7 | N/A |
| GCP | CentOS 7 |  N/A  |
| OpenStack | CentOS 7 |  N/A |

[Comment]: <> (Not sure about the AWS prewarmed images.)

Since the standard default images may not fit the requirements of some users (for example when user requirements include custom OS hardening, custom libraries, custom tooling, and so on), Cloudbeak allows you to use your own **custom images**.

In order to use your own custom base images you must:

1. Build your custom images  
2. Prepare the image catalog JSON file  
3. Register your custom images with Cloudbreak   
4. Select a custom image when creating a cluster  

Only base images can be created and registered as custom images. Do not create or register prewarmed images using the instructions below. 


### Building Custom Images

Refer to [Custom Images for Cloudbreak](https://github.com/hortonworks/cloudbreak-images). This repository includes instructions and scripts to help you build custom images. Once you have the images, refer to the documentation below for information on how to create an image catalog, and register and use these images with Cloudbreak.


### Preparing the Image Catalog 

Once you've built the custom images, create the `custom-image-catalog.json` file and save it on your Cloudbreak machine in the `/var/lib/cloudbreak-deployment/etc` directory. The etc directory does not exist by default so you must create it. 

The structure of the image catalog JSON file file and an example image catalog JSON file are provided below. 
 

#### Structure of the Image Catalog JSON File 

The image catalog JSON file includes the following two high-level sections: 

* `images`: Contains information about the created images. The burned images are stored in the `base-images` section.  
* `versions`: Contains the `cloudbreak` entry, which includes mapping between Cloudbreak versions and the image identifiers of burned images available for these Cloudbreak versions.   

The `base-images` section stores one or more image "records". Every image "record" must contain the date, description, images, os, os_type, and uuid fields.

| Field | Description |
|---|---|
| date | Date for your image catalog entry. |
| description | Description for your image catalog entry. |
| images | the `images` field must contain a map that contains the image sets by a provider and an image set must store the virtual machine image IDs by the related region of the provider. The virtual machine image IDs come from the result of the image burning process and must be an existing identifier of a virtual machine image on the related provider side. For the providers which use global rather than per-region images, the region should be replaced with **`default`**, as the example image catalog JSON illustrates. |
| os | The operating system used in the image. |
| os_type | |
| uuid | The `uuid` field must be a unique identifier within the file. You can generate it or select it manually. The utility `uuidgen` available from your command line is a convenient way to generate a unique ID. |

The `versions` section includes a single "cloudbreak" entry, which maps the uuids to a specific Cloudbreak version:

| Field | Description |
|---|---|
| images | Image `uuid`, same as the one that you specified in the `base-images` section. |
| versions | The Cloudbreak version(s) for which you would like to use the images. |


#### Example Image Catalog JSON File 

Here is an example JSON image catalog file that includes a custom base image:

* For AWS, Azure, Google, and OpenStack  
* That is using CentOS 7 operating system   
* Has a unique ID of f6e778fc-7f17-4535-9021-515351df3692  
* Is available to Cloudbreak 2.1.0  

You can also download it from [here](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/custom-image-catalog.json).


<pre>
{
  "images": {
    "base-images": [
      {
        "date": "2017-10-13",
        "description": "Cloudbreak official base image",
        "images": {
          "aws": {
            "ap-northeast-1": "ami-78e9311e",
            "ap-northeast-2": "ami-84b613ea",
            "ap-southeast-1": "ami-75226716",
            "ap-southeast-2": "ami-92ce23f0",
            "eu-central-1": "ami-d95be5b6",
            "eu-west-1": "ami-46429e3f",
            "sa-east-1": "ami-86d5abea",
            "us-east-1": "ami-51a2742b",
            "us-west-1": "ami-21ccfe41",
            "us-west-2": "ami-2a1cdc52"
          }
        },
        "os": "amazonlinux",
        "os_type": "redhat6",
        "uuid": "44b140a4-bd0b-457d-b174-e988bee3ca47"
      },
      {
        "date": "2017-10-13",
        "description": "Cloudbreak official base image",
        "images": {
          "azure": {
            "Australia East": "https://hwxaustraliaeast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Australia South East": "https://hwxaustralisoutheast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Brazil South": "https://sequenceiqbrazilsouth2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Canada Central": "https://sequenceiqcanadacentral.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Canada East": "https://sequenceiqcanadaeast.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Central India": "https://hwxcentralindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Central US": "https://sequenceiqcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East Asia": "https://sequenceiqeastasia2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East US": "https://sequenceiqeastus12.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "East US 2": "https://sequenceiqeastus22.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Japan East": "https://sequenceiqjapaneast2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Japan West": "https://sequenceiqjapanwest2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Korea Central": "https://hwxkoreacentral.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Korea South": "https://hwxkoreasouth.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "North Central US": "https://sequenceiqorthcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "North Europe": "https://sequenceiqnortheurope2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "South Central US": "https://sequenceiqouthcentralus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "South India": "https://hwxsouthindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "Southeast Asia": "https://sequenceiqsoutheastasia2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "UK South": "https://hwxsouthuk.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "UK West": "https://hwxwestuk.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West Central US": "https://hwxwestcentralus.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West Europe": "https://sequenceiqwesteurope2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West India": "https://hwxwestindia.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West US": "https://sequenceiqwestus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd",
            "West US 2": "https://hwxwestus2.blob.core.windows.net/images/hdc-hdp--1710161226.vhd"
          },
          "gcp": {
            "default": "sequenceiqimage/hdc-hdp--1710161226.tar.gz"
          },
          "openstack": {
            "default": "hdc-hdp--1710161226"
          }
        },
        "os": "centos7",
        "os_type": "redhat7",
        "uuid": "f6e778fc-7f17-4535-9021-515351df3691"
      }
    ]
},
  "versions": {
    "cloudbreak": [
      {
        "images": [
          "44b140a4-bd0b-457d-b174-e988bee3ca47",
          "f6e778fc-7f17-4535-9021-515351df3692"
        ],
        "versions": [
          "2.1.0"
        ]
      }
    ]
  }
}
</pre>


### Register Custom Images

Using the custom image JSON you created, register that JSON with your Cloudbreak instance. 

**Steps**

1. On the Cloudbreak machine, navigate to the Cloudbreak deployment directory:  
    cd /var/lib/cloudbreak-deployment    
2. Edit the Profile file:     
    vi Profile      
2. Add `export CB_IMAGE_CATALOG_URL` to the file and set it to the location of your JSON file which declares your custom images. Its value can be:
    * A **URL** of a remote file that is reachable by Cloudbreak through HTTP/HTTPS or    
    * A **PATH** to the file, relative to the /var/lib/cloudbreak-deployment/etc directory on the Cloudbreak host. For example: `export CB_IMAGE_CATALOG_URL=custom-image-catalog.json`    
3. Save the Profile file.
4. Restart Cloudbreak.


### Select a Custom Image When Creating a Cluster

Once you have registered the custom images you can use them when creating a cluster. 

In the web UI, this option called **Choose OS Type** is available in the advanced **General Configuration** section of the wizard, allowing you to select the OS and the base image to use. You can leave the default entries for the Ambari and HDP repositories, or you can customize to point to specific versions of Ambari and HDP that you want to use for the cluster.

<a href="../images/cb-images.png" target="_blank" title="click to enlarge"><img src="../images/cb-images.png" width="650" title="Cloudbreak UI"></a>

In the CLI, to use the custom image when creating a cluster, add the "ImageId" parameter and set it to the uuid of the image(s) registered. For example: 

<a href="../images/images-cli.png" target="_blank" title="click to enlarge"><img src="../images/images-cli.png" width="650" title="CLI JSON file showing images entry"></a> 


### Reverting to Default Images

To revert to the default images, remove the `export CB_IMAGE_CATALOG_URL=custom-image-catalog.json` entry that you added when [registering custom images](#register-custom-images) and then restart Cloudbreak.  



