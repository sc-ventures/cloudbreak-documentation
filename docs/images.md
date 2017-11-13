## Custom Images

By default, Cloudbreak launches clusters from an image that includes default configuration and default tooling for provisioning. These are considered the Standard default images.

In some cases, these default images might not fit the requirements of users: for example when they need custom OS hardening, libraries, tooling, and so on. In such cases, the user would like to start their clusters from their own custom image.

### Building Custom Images

Refer to [Custom Images for Cloudbreak](https://github.com/hortonworks/cloudbreak-images). This repository includes instructions and scripts to help you build custom images. Once you have the images, refer to the documentation below for information on how to register and use these images with Cloudbreak.

## Registering Custom Images

Register your custom image(s) in Cloudbreak by export the `CB_IMAGE_CATALOG_URL` variable in your Profile file that points to an image catalog `JSON` file that
declare your custom images. It's value could be an **URL** of a remote file that is reachable by Cloudbreak through `HTTP/HTTPS` or the **path** of the file
relatively to the `/var/lib/cloudbreak-deployment/etc` directory on the Cloudbreak host.
The`etc` directory does not exist by default so you need to create it.

> Important: If you register images after Cloudbreak has been started, you may need to wait until the cached data will be refreshed.
Cloudbreak stores the image catalog related data only for 15 minutes.

### Structure of the image catalog JSON
The `JSON` file has two main sections the `images` that contains information about the burned images and the `versions` that contains the mappings between
Cloudbreak versions and the available image identifiers for them in the list under the `cloudrbeak` key.

The burned images are stored under the `images` main section in 3 categories based on the platform that is pre-warmed:
- `base-images` section of the not pre-warmed images
- `hdf-images` contains the HDF related images
- `hdp-images` contains the HDP related images

The image 'records' in the 3 categories are almost the same except that the `base-images` section's records doesn't contain the version and repo information
of the pre-warmed Ambari and platform (**HDF/HDP**). So the platform related images must contain the `version`, `repo` and `stack-details` fields. The `repo`
field must contain the Ambari's installer package **URL** by the given os-type. The `stack-details` field must contain a `version` field that is identifies
the version of the pre-warmed platform and a `repo` field that contains the necessary package information for the platform.

Every image 'record' must contain the `os`, `date`, `uuid` and `images` fields. The `uuid` field should be a unique identifier within the file and
the `images` field must contain a map that contains the image sets by a provider and an image set must store the virtual machine image ids by the related
region of the provider. The virtual machine image ids come from the result of the image burning process and must be an existing identifier of a virtual
machine image on the related provider side.
>The providers which use global images the region should be **`default`**, as the example `JSON` describes.

**Example image catalog:**
```
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
```
