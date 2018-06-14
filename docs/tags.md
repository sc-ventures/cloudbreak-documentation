## Tagging resources 

When you manually create resources in the cloud, you have an option to add custom tags that help you track these resources. Likewise, when creating clusters, you can instruct Cloudbreak to tag the cloud resources that it creates on your behalf. The tags added during cluster creation are displayed in your cloud account on the resources that Cloudbreak provisioned for your clusters. 

You can use tags to categorize your cloud resources by purpose, owner, and so on. Tags come in especially handy when you are using a corporate AWS account and you want to quickly identify which resources belong to your cluster(s). In fact, your corporate cloud account admin may require you to tag all the resources that you create, in particular resources, such as VMs, which incur charges.


### Add tags when creating a cluster

You can tag the cloud resources used for a cluster by providing custom tag names and values when creating a cluster via UI or CLI. In the Cloudbreak UI, this option is available in the create cluster wizard, in the advanced **General Configuration** page > **Tags** section. 

It is not possible to add tags via Cloudbreak after your cluster has been created.  

[comment]: <> (Commenting out the content which does not apply but we may want to add it in the future.)
[comment]: <> (When you clone your cluster, all tags associated with the source cluster will be added to the template of the clone.)  
[comment]: <> (When you save a cluster template, all tags will be saved as part of the template, and they will be listed on the cluster template page.)    


To learn more about tags and their restrictions, refer to the cloud provider documentation. 

**Related links**  
[Tags on AWS](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)    
[Tags on Azure](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags)  
[Labels on GCP](https://cloud.google.com/resource-manager/docs/using-labels)  
[Tags on OpenStack](https://docs.openstack.org/mitaka/networking-guide/ops-resource-tags.html)  

### Add tags in Profile (AWS)

In order to differentiate launched instances, you can optionally define custom tags for your AWS resources deployed by Cloudbreak. 

* If you want just one custom tag for your CloudFormation resources, set this variable in the `Profile`:

    ```export CB_AWS_DEFAULT_CF_TAG=mytagcontent```

    In this example, the name of the tag will be `CloudbreakId` and the value will be `mytagcontent`.

* If you prefer to customize the tag name, set this variable:

    ```export CB_AWS_CUSTOM_CF_TAGS=mytagname:mytagvalue```

    In this example the name of the tag will be `mytagname` and the value will be `mytagvalue`. 

* You can specify a list of tags with a comma separated list: 

    ```export CB_AWS_CUSTOM_CF_TAGS=tag1:value1,tag2:value2,tag3:value3```
    
    
    
