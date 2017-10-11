## Manage Your Account   

You can manage your Cloudbreak account from the Cloudbreak UI by clicking **account** in the top right corner.

### Get Usage Report

You can generate a usage report for all cluster resources related to your Cloudbreak instance. To generate a report, perform these steps.

**Steps**

1. From the Cloudbreak dashboard, click on **account** in the top right corner.  
2. Navigate to the **usage report** tab.  
1. Select the range of dates for which you want the report.  
2. Select a specific user or **all**.  
3. Select a region.  
4. Click **generate** to generate the report.  
5. The report will be displayed, including instance types and running time for each node group.   


### Check Account Details 

To view your account details, navigate to the **account details** tab. 


### Manage Users 

You can manage existing users (activate and deactivate) and invite new users to use your Cloudbreak deployment from the **manage users** tab.


#### Invite a New User

<div class="danger">
    <p class="first admonition-title">Note</p>
    <p class="last">SMTP is not supported by Azure, so in order to use this feature you have to configure an external email service using the steps described in the <a href="http://sequenceiq.com/cloudbreak-docs/latest/configuration/#smtp"> Cloudbreak</a> documentation.</p>
</div>

To invite a new user, perform these steps.

**Steps**

1. From the Cloudbreak dashboard, click on **account** in the top right corner.  
2. Navigate to the **manage users** tab.   
3. Click on **+invite new user**. 
1. Specify the email address and the scope of access for the user. 
2. Click **+invite new user** and you will see the name of the new user added to the user list.
2. Click on the name of the user.
3. Click **activate**. 


#### Activate an Existing User

To activate an existing user, perform these steps.

**Steps**

1. From the Cloudbreak dashboard, click on **account** in the top right corner.  
2. Navigate to the **manage users** tab. 
1. Click on the name of the user.
2. Click **activate**. 


#### Deactivate an Existing User

To deactivate an existing user, perform these steps.

**Steps**

1. From the Cloudbreak dashboard, click on **account** in the top right corner.  
2. Navigate to the **manage users** tab. 
1. Click on the name of the user.
2. Click **deactivate**. 

>>>>TO-DO: The old docs had something about "Security Scopes". Not sure if this will exist in the new UI? 
 
