## Register a proxy   

Cloudbreak allows you to save your existing proxy configuration information so that you can provide the proxy information to the clusters that you create with Cloudbreak. The steps are:       

1. Register your proxy in Cloudbreak web UI or CLI.   
2. Once the proxy has been registered with Cloudbreak, it will show up in the list of available proxies when creating a cluster under advanced **External Sources** > **Configure Proxy**.  


**Steps** 

1. From the navigation pane, select **External Sources** > **Proxy Configurations**.  
2. Select **Register Proxy Configuration**.    
5. Provide the following information:

    | Parameter | Description | Example |
    |:---|:---|:---|
    | Name | Enter the name to use when registering this database to Cloudbreak. This is **not** the database name. | my-proxy |
    | Description | Provide description. | |
    | Protocol | Select HTTP or HTTPS. | HTTPS|
    | Server Host| Enter the URL of your proxy server host. | 10.0.2.237 |
    | Server Port | Enter proxy server port. | 3128 |
    | Username | Enter the username for the proxy. | testuser |
    | Password | Enter the password for the proxy. | MyPassword123 |

6. Click **REGISTER** to save the configuration. 

7. The proxy will now show up when creating a cluster under advanced **External Sources** > **Configure Proxy**. You can select it each time you would like to use it for a cluster.  
  
