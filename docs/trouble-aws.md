## Troubleshooting Cloudbreak on AWS

> Check out [HDCloud](http://hortonworks.github.io/hdp-aws/trouble/index.html) troubleshooting docs.

### Unable to Create an IAM Role(s) for Cloudbreak 

Most corporate AWS users are unable to create AWS roles. You may have to contact your AWS admin to create the role(s) for you. 

### Cluster Fails with Permissions Related Error

Make sure that the policy attached to CredentialRole includes all actions defined in [CredentialRole](https://docs.hortonworks.com/HDPDocuments/Cloudbreak/Cb-doc-resources/cb-policy.json).  