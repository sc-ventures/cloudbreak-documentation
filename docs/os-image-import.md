## Manually import HDP and HDF images to OpenStack

An OpenStack administrator must perform these steps to add the Cloudbreak deployer image to your OpenStack deployment. 

> Importing prewarmed and base HDP and HDF images is no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create a cluster.

### Import HDP prewarmed image

These steps are no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDP cluster. If you would like to import HDP prewarmed images manually (instead of having them imported), you can do this by using the following steps.

**Steps**

1. Download the latest HDP image to your local machine:

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-26-1803301748.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp-26-1803301748.img
export CB_LATEST_IMAGE_NAME=cb-hdp-26-1803301748.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images.


### Import HDF prewarmed image

These steps are no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDF cluster. If you would like to import HDP prewarmed images manually (instead of having them imported), you can do this by using the following steps.

**Steps**

1. Download the latest HDF image to your local machine:

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp-26-1803301748.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp-26-1803301748.img
export CB_LATEST_IMAGE_NAME=cb-hdp-26-1803301748.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images. 


### Import HDP/HDF base image

These steps are no longer required, because if these images are not imported manually, Cloudbreak will import them once you attempt to create an HDF cluster. If you would like to import HDP/HDF base images manually (instead of having them imported), you can do this by using the following steps.

**Steps**

1. Download the latest HDP image to your local machine:

    <pre><small>curl -O https://public-repo-1.hortonworks.com/HDP/cloudbreak/cb-hdp--1803211203.img</small></pre>

2. Set the following environment variables for the OpenStack image import:

    <pre><small>export CB_LATEST_IMAGE=cb-hdp--1803211203.img
export CB_LATEST_IMAGE_NAME=cb-hdp--1803211203.img
export OS_USERNAME=your_os_user_name
export OS_AUTH_URL=your_authentication_url
export OS_TENANT_NAME=your_os_tenant_name</small></pre>

3. Import the new image into your OpenStack:

    <pre><small>glance image-create --name "$CB_LATEST_IMAGE_NAME" --file "$CB_LATEST_IMAGE" --disk-format qcow2 --container-format bare --progress</small></pre>

After performing the import, you should be able to see the Cloudbreak image among your OpenStack images.

