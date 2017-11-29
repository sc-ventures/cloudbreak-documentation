## Disable Providers 

If you are planning to use Cloubdreak with a specific cloud provider or a specific set of cloud providers, you may want to disable the remaining providers. For example, if you are planning to use Cloubdreak with Azure only, you may want to disable AWS, Google Cloud, and OpenStack. 

**Steps**

1. Navigate to the Cloudbreak deployment directory and edit Profile. For example:

    <pre>cd /var/lib/cloudbreak-deployment/
vi Profile</pre>

2. Add the following entry, setting it to the provider that you  would like to see. For example, if you would like to see Azure only, set this to "AZURE":

    <pre>export CB_ENABLEDPLATFORMS=AZURE</pre>

    Accepted values are:
    
    * AZURE
    * AWS
    * GCP
    * OPENSTACK

    Any combination of platforms can be used; for example if you would like to see AWS and OpenStack, then use:
    
    <pre>export CB_ENABLEDPLATFORMS=AWS,OPENSTACK</pre>

    If you want to reverse the change and see all providers, then either delete CB_ENABLEDPLATFORMS from the Profile or add the following: 
    
    <pre>export CB_ENABLEDPLATFORMS=AZURE,AWS,GCP,OPENSTACK</pre>
    
3. Restart Cloudbreak by using `cbd restart`.      

