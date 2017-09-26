## Securing Cloudbreak After Launch

Cloudbreak comes with default settings designed for easy first experience rather than strict security. To secure Cloudbreak, follow these recommendations. 

### Restricting Inbound Access 

We recommend that you block all communication ports except 443 on the firewall or security group (depending on the provider). 

If you have to log in to the Cloudbreak host remotely, use the SSH port (usually 22).

>>>>TO-DO: Is the step above up-to-date? I think ports 80 and 22 is also open. 

### Configuring the Profile 

Before starting Cloudbreak for the first time, configure the Profile file as directed below. Changes are applied during startup so a restart (`cbd restart`) is required after each change.

1. Execute the following command in the directory where you want to store Cloudbreak-related files:

    <pre>
echo export PUBLIC_IP=[the ip or hostname to bind] > Profile
</pre>

    >>>>TO-DO: Do you mean that this needs to be executed in the deployment directory? Or?

2. After you have a base Profile file, add the following custom properties to it:

    <pre>
export UAA_DEFAULT_SECRET='[custom secret]'
export UAA_DEFAULT_USER_EMAIL='[default admin email address]'
export UAA_DEFAULT_USER_PW='[default admin password]'
export UAA_DEFAULT_USER_FIRSTNAME='[default admin first name]'
export UAA_DEFAULT_USER_LASTNAME='[default admin last name]'
</pre>

    Cloudbreak has additional secrets which by default inherit their values from `UAA_DEFAULT_SECRET`. Instead of using the default, you can define different values in the Profile for each of these service clients:

    <pre>
export UAA_CLOUDBREAK_SECRET='[cloudbreak secret]'
export UAA_PERISCOPE_SECRET='[auto scaling secret]'
export UAA_ULUWATU_SECRET='[web ui secret]'
export UAA_SULTANS_SECRET='[authenticator secret]'
</pre>

    You can change these secrets at any time, except `UAA_CLOUDBREAK_SECRET` which is used to encrypt sensitive information at database level. 
    
    >>>>TO-DO: The info below is explained in a way that is confusing. Can you rephrase? 

    `UAA_DEFAULT_USER_PW` is stored in plain text format, but if `UAA_DEFAULT_USER_PW` is missing from the Profile, it gets a default value. Because default password is not an option, if you set an empty password explicitly in the Profile Cloudbreak deployer will ask for password all the time when it is needed for the operation.

    <pre>
export UAA_DEFAULT_USER_PW=''
</pre>

    In this case, Cloudbreak deployer wouldn't be able to add the default user, so you have to do it manually by executing the following command:

    <pre>
cbd util add-default-user
</pre>


### Adding SSL Certificate for Cloudbreak 


