

## SmartSense Telemetry

Help us make a better product by opt'ing in to automatically send information to Hortonworks. This includes enabling
Hortonworks SmartSense and sending performance and usage info. As you use the product,
SmartSense measures and collects information and then sends these information bundles to Hortonworks.

### How to Disable

#### Disable Bundle Upload for the Cloud Controller and New Clusters

<div class="danger">
    <p class="first admonition-title">Important</p>
    <p class="last">
    Do not perform these steps when you have clusters currently in the process of being deployed.
    Wait for all clusters to be deployed.</p>
</div>


1. SSH into the cloud controller host.

1. Edit `/var/lib/cloudbreak-deployment/Profile`.

2. Change `CB_SMARTSENSE_CONFIGURE` to `false`:<br>
    <pre><code>export CB_SMARTSENSE_CONFIGURE=false</code></pre>

2. Restart the cloud controller:<br>
    <pre><code>cd /var/lib/cloudbreak-deployment
cbd restart</code></pre>
    

#### Disable Bundle Upload for an Existing Cluster

1. SSH into the Master node for the cluster.

1. Edit `/etc/hst/conf/hst-server.ini`.

2. Change `[gateway]` configuration to `false`:<br>
    <pre><code>[gateway]
enabled=false</code></pre>

2. Restart the SmartSense Server:
    <pre><code>hst restart</code></pre>
    
3. (Optional) Disable SmartSense daily bundle capture:

    * SmartSense is scheduled to capture a telemetry bundle daily. With the bundle upload disabled, the bundle will still
    be captured but just saved locally (i.e. not uploaded).
    * To disable the bundle capture, execute the following:
    <pre><code>hst capture-schedule -a pause</code></pre>

3. Repeat on all existing clusters.


