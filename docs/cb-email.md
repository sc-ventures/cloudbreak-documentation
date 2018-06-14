
## Configure SMTP email notifications 

If you want to configure email notification, configure SMTP parameters in your `Profile`. 

> In order to use this configuration, your email server must use SMTP. 

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

For example: 

```
export CLOUDBREAK_SMTP_SENDER_USERNAME='myemail@gmail.com'  
export CLOUDBREAK_SMTP_SENDER_PASSWORD='Mypassword123'  
export CLOUDBREAK_SMTP_SENDER_HOST='smtp.gmail.com'  
export CLOUDBREAK_SMTP_SENDER_PORT=25  
export CLOUDBREAK_SMTP_SENDER_FROM='myemail@gmail.com'  
export CLOUDBREAK_SMTP_AUTH=true  
export CLOUDBREAK_SMTP_STARTTLS_ENABLE=true  
export CLOUDBREAK_SMTP_TYPE=smtp  
```

> The example assumes that you are using gmail. You should use the settings appropriate for your SMTP server.

If your SMTP server uses SMTPS, you must set the protocol in your `Profile` to smtps:

```
export CLOUDBREAK_SMTP_TYPE=smtps
```


