## Registering a Proxy   

Cloudbreak allows you to save your existing proxy configuration information so that you can provide the proxy information to the clusters that you create with Cloudbreak. The steps are:       

1. Register your proxy in Cloudbreak web UI or CLI.   
2. Once the poxy has been registered with Cloudbreak, it will show up in the list of available proxies when creating a cluster under advanced **External Sources** > **Configure Proxy**.  


### Register a Proxy  

Follow these steps to register your proxy with Cloudbreak and reuse the proxy configuration for clusters created with Cloudbreak. 

**Steps** 

1. From the navigation pane, select **External Sources** > **Proxy Configurations**.  
2. Select **Register Proxy Configuration**.    
5. Provide the following information:

    | Parameter | Description |
    |:---|:---|
    | Name | Enter the name to use when registering this database to Cloudbreak. This is **not** the database name. |
    | Description | Provide description. | 
    | Protocol | Select HTTP or HTTPS. |
    | Server Host| Enter the URL of your proxy server host. |
    | Server Port | Enter proxy server port. |
    | Username | Enter the username for the proxy. |
    | Password | Enter the password for the proxy. |

6. Click **REGISTER** to save the configuration. The proxy will now show up when creating a cluster under advanced **External Sources** > **Configure Proxy**.
  
