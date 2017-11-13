## Custom Images

By default, Cloudbreak launches clusters from **standard default images**, which include default configuration and default tooling. These images are available by default for each supported provider and region. 

Since the standard default images may not fit the requirements of some users (for example when user requirements include custom OS hardening, custom libraries, custom tooling, and so on), Cloudbeak allows you to use your own **custom images**.

In order to use your own custom images you must:

1. Build your custom images 
2. Prepare the image catalog JSON file 
3. Register your custom images with Cloudbreak  
4. Select a custom image when creating a cluster


### Building Custom Images

Refer to [Custom Images for Cloudbreak](https://github.com/hortonworks/cloudbreak-images). This repository includes instructions and scripts to help you build custom images. Once you have the images, refer to the documentation below for information on how to create an image catalog, and register and use these images with Cloudbreak.


### Preparing the JSON File with Image Catalog 

Once you've built the custom images, you must create an image catalog JSON file. An example is available [here](#example-image-catalog-json-file).  

[Comment]: <> (I am still not clear on what the different scenarios are and what exactly to replace in this file for which scenario. We need to describe which exact sections and entries to update for specific customer use cases.)


#### Structure of the Image Catalog JSON File 

The image catalog JSON file includes the following two high-level sections: 

* `images`: Contains information about the burned images.   
* `versions`: Contains the mappings between Cloudbreak versions and the image identifiers of burned images available for these versions in the list under the `cloudrbeak` key.  

[Comment]: <> (What do you mean by "list under the `cloudrbeak` key"?)

The burned images are stored in the `images` section in three categories, based on the platform that is pre-warmed:

[Comment]: <> (What do you mean by "non pre-warmed" images? What is this section used for?)

* `base-images`: Contains the non pre-warmed images  
* `hdf-images`: Contains the HDF related images  
* `hdp-images`: Contains the HDP related images  

[Comment]: <> (From the customer perspective, when to use each of these sections?) 

Each of these sections stores image "records". Every image "record" must contain the `os`, `date`, `uuid` and `images` fields. The `uuid` field must be a unique identifier within the file, and the `images` field must contain a map that contains the image sets by a provider and an image set must store the virtual machine image IDs by the related region of the provider. The virtual machine image IDs come from the result of the image burning process and must be an existing identifier of a virtual machine image on the related provider side.  

[Comment]: <> (And also optional description?)

In addition, the platform-related images (`hdf-images` and `hdp-images`) must contain the version and repo information of the pre-warmed Ambari and platform (**HDF/HDP**) images: the `version`, `repo`, and `stack-details` fields. The `repo` field must contain the Ambari's installer package **URL** by the given os-type. The `stack-details` field must contain a `version` field, which identifies the version of the pre-warmed platform and, a `repo` field, which contains the necessary package information for the platform. 

For the providers which use global rather than per-region images, the region should be replaced with **`default`**, as the example image catalog JSON illustrates.


#### Example Image Catalog JSON File 

<pre>
{
  "images": {
    "base-images": [
      {
        "date": "2017-10-13",
        "description": "Cloudbreak official base image",
        "images": {
          "mock": {
            "default": "mockimage/hdc-hdp--1710161226.tar.gz"
          }
        },
        "os": "centos7",
        "uuid": "f6e778fc-7f17-4535-9021-515351df3691"
      },
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
          },
          "gcp": {
            "default": "sequenceiqimage/hdc-hdp--1710161226.tar.gz"
          },
          "openstack": {
            "default": "hdc-hdp--1710161226"
          }
        },
        "os": "centos7",
        "uuid": "f6e778fc-7f17-4535-9021-515351df3692"
      }
    ],
    "hdf-images": [],
    "hdp-images": [
      {
        "date": "2017-08-07",
        "description": "Cloudbreak official image",
        "images": {
          "aws": {
            "ap-northeast-1": "ami-e621ca80",
            "ap-northeast-2": "ami-6a29f004",
            "ap-southeast-1": "ami-3e01985d",
            "ap-southeast-2": "ami-ed253c8e",
            "eu-central-1": "ami-76812f19",
            "eu-west-1": "ami-8d3dcbf4",
            "sa-east-1": "ami-1897e174",
            "us-east-1": "ami-5428072f",
            "us-west-1": "ami-36351e56",
            "us-west-2": "ami-b7d435cf"
          },
          "azure": null,
          "gcp": null,
          "openstack": null
        },
        "os": "centos6",
        "uuid": "2.4.2.2-1-aec0d3ca-e04a-47bf-9970-d649b0c579b9-2.5.0.1-265",
        "stack-details": {
          "repo": {
            "stack": {
              "redhat6": "http://private-repo-1.hortonworks.com/HDP/centos6/2.x/updates/2.5.0.1-265",
              "repoid": "HDP-2.5"
            },
            "util": {
              "redhat6": "http://private-repo-1.hortonworks.com/HDP-UTILS-1.1.0.21/repos/centos6",
              "repoid": "HDP-UTILS-1.1.0.21"
            }
          },
          "version": "2.5.0.1-265"
        },
        "repo": {
          "redhat6": "http://private-repo-1.hortonworks.com/ambari/centos6/2.x/updates/2.4.2.2-1/"
        },
        "version": "2.4.2.2-1"
      }
    ]
  },
  "versions": {
    "cloudbreak": [
      {
        "images": [
          "f6e778fc-7f17-4535-9021-515351df3691",
          "f6e778fc-7f17-4535-9021-515351df3692",
          "2.4.2.2-1-aec0d3ca-e04a-47bf-9970-d649b0c579b9-2.5.0.1-265"
        ],
        "versions": [
          "2.1.0-rc.1"
        ]
      }
    ]
  }
}
</pre>



### Register Custom Images

Register your custom images in Cloudbreak by using the following steps. In general, you must edit your Profile file, by adding `export CB_IMAGE_CATALOG_URL=` pointing to the JSON file which contains your image catalog. 

You may register the images either before starting Cloudbreak or after Cloudbreak is already running. 

**Steps**

1. SSH to the Cloudbreak VM and navigate to the Cloudbreak deployment directory, which usually is `/var/lib/cloudbreak-deployment` and edit the Profile file.
2. To register your custom images, add `export CB_IMAGE_CATALOG_URL=` to your profile and set it to the location of your `JSON` file which declares your custom images. Its value can be:  
    * A **URL** of a remote file that is reachable by Cloudbreak through HTTP/HTTPS or  
    * A **path** to the file, relative to the `/var/lib/cloudbreak-deployment/etc` directory on the Cloudbreak host. The`etc` directory does not exist by default so you must create it. For example If you have an image catalog file called "custom-catalog.json" located in the `/var/lib/cloudbreak-deployment/etc` directory, you can simply enter in your Profile `export CB_IMAGE_CATALOG_URL=custom-catalog.json`.  
3. If you register images after Cloudbreak has been started, you may need to wait until the cached data is refreshed.
Cloudbreak stores the image catalog related data only for 15 minutes.

[Comment]: <> ("Cloudbreak stores the image catalog related data only for 15 minutes." What do you mean? Do you mean that if I add the "export CB_IMAGE_CATALOG_URL" when Cloudbeak is already running, after 15 minutes the images should be refreshed?? Or do I need to issue "cbd restart"??)
