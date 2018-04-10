
## Customizing Cloudbreak Profile file  

Cloudbreak deployer configuration is based on environment variables.  

During startup, Cloudbreak deployer tries to determine the underlying infrastructure and then sets required environment variables with appropriate default values. If these environment variables are not sufficient for your use case, you can set additional environment variables in your `Profile` file. 


### Set Profile variables

To set environment variables relevant for Cloudbreak Deployer, add them to a file called `Profile` located in the Cloudbreak deployment directory (typically `/var/lib/cloudbreak-deployment`).

The `Profile` file is sourced, so you can use the usual syntax to set configuration values:

```
export MY_VAR=some_value
export MY_OTHER_VAR=another_value 
```

After changing a property, you must regenerate the config file and restart the application by using `cbd restart`.


### Check available Profile variables

To see all available environment variables with their default values, use:

```
cbd env show
```

### Create environment-specific profiles

If you would like to use a different versions of Cloudbreak for prod and qa profile, you must create two environment specific configurations that can be sourced. For example:

* Profile.prod  
* Profile.qa   

For example, to create and use a prod profile, you need to:

1. Create a file called `Profile.prod`  
2. Write the environment-specific `export DOCKER_TAG_CLOUDBREAK=0.3.99` into `Profile.prod` to specify Docker image.  
3. Set the environment variable: `CBD_DEFAULT_PROFILE=prod`  

To use the prod specific profile once, set:  

<pre>CBD_DEFAULT_PROFILE=prod cbd some_commands</pre>
    
To permanently use the prod profile, set `export CBD_DEFAULT_PROFILE=prod` in your `.bash_profile`.


### Secure the Profile file 

Before starting Cloudbreak for the first time, configure the Profile file as directed below. Changes are applied during startup so a restart (`cbd restart`) is required after each change.

1. Execute the following command in the directory where you want to store Cloudbreak-related files:

    <pre>
echo export PUBLIC_IP=[the ip or hostname to bind] > Profile
</pre>

[comment]: <> (TO-DO: Do you mean that this needs to be executed in the deployment directory? Or?)

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
    
[comment]: <> (TO-DO: The info below is explained in a way that is confusing. Can you rephrase?) 

    `UAA_DEFAULT_USER_PW` is stored in plain text format, but if `UAA_DEFAULT_USER_PW` is missing from the Profile, it gets a default value. Because default password is not an option, if you set an empty password explicitly in the Profile Cloudbreak deployer will ask for password all the time when it is needed for the operation.

    <pre>
export UAA_DEFAULT_USER_PW=''
</pre>

    In this case, Cloudbreak deployer wouldn't be able to add the default user, so you have to do it manually by executing the following command:

    <pre>
cbd util add-default-user
</pre>

For more information about setting environment variables in Profile, refer to [Configure Profile Variables](#configure-profile-variables).


### Change SMTP parameters

If you want to change SMTP parameters, add them your `Profile`.

The default values of the SMTP parameters are:

```
export CLOUDBREAK_SMTP_SENDER_USERNAME=
export CLOUDBREAK_SMTP_SENDER_PASSWORD=
export CLOUDBREAK_SMTP_SENDER_HOST=
export CLOUDBREAK_SMTP_SENDER_PORT=25
export CLOUDBREAK_SMTP_SENDER_FROM=
export CLOUDBREAK_SMTP_AUTH=true
export CLOUDBREAK_SMTP_STARTTLS_ENABLE=true
export CLOUDBREAK_SMTP_TYPE=smtp
```

If your SMTP server uses SMTPS, you must set the protocol in your `Profile` to smtps:

```
export CLOUDBREAK_SMTP_TYPE=smtps
```

### Configure Consul 

Cloudbreak uses [Consul](https://www.consul.io/) for DNS resolution. All Cloudbreak related services are registered as someservice.service.consul.

Consul’s built-in DNS server is able to fallback on another DNS server. This option is called `-recursor`. Cloudbreak Deployer first tries to discover the DNS settings of the host by looking for nameserver entry in the `/etc/resolv.conf` file. If it finds one, consul will use it as a recursor. Otherwise, it will use `8.8.8.8`.

For a full list of available consul config options, refer to [Consul documentation](https://www.consul.io/docs/agent/options.html).

To pass any additional Consul configuration, define the `DOCKER_CONSUL_OPTIONS` variable in the Profile file.


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
    
    
    