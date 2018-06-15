## Manually import HDP and HDF images to OpenStack

An OpenStack administrator can perform these steps to add the Cloudbreak deployer image to your OpenStack deployment. Perform these steps for each image. 

> Importing prewarmed and base HDP and HDF images is no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create a cluster.

The following images can be imported:

| Image | Operating system | Location |
|---|---|---|
| Prewarmed HDP 2.6 image | centos7 | http://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-26-1805171052.img |
| Prewarmed HDF 3.1 image | centos7 | http://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-31-1805251001.img |
| Base image | centos7 | http://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp--1806131251.img |
| Base image | ubuntu | http://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp--1805252042.img |

**Steps**

1. Download the image to your local machine. For example: 

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-26-1805171052.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp-26-1805171052.img
export CB_LATEST_IMAGE_NAME=cb-hdp-26-1805171052.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images.

