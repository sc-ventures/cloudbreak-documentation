
## Getting Started with the CLI   

### Get Started with the CLI 

After [installing](cli-install.md#install-the-cli) and [configuring](cli-install.md#configure-the-cli) the CLI, you can use it to perform the same tasks as are available in the Cloudbreak UI: create and manage clusters, credentials, blueprints, and recipes.

**Steps**

1. Before you start using the CLI, familiarize yourself with Cloudbreak concepts and the Cloudbreak web UI. 

2. If you haven't already, create at least one Cloudbreak credential by using Cloudbreak UI or the [credential create](cli-reference.md#credential-create) command. 

3. To create a cluster, you must first generate a JSON skeleton. Although it is possible to generate it by using the [cluster generate-template](cli-reference.md#cluster-generate-template) command, it is easiest to obtain it from the Cloudbreak UI, as described in [Obtain Cluster JSON Template from the UI](#obtain-cluster-json-template-from-the-ui).

4. Save the template in the JSON format and edit it if needed.

5. Once your JSON file is ready, you can use it to create a cluster via the [cluster create](cli-reference.md#cluster-create) command.

6. Once your cluster is running, use can use the CLI to manage and monitor your cluster. For a full list of commands, refer to [CLI Reference](cli-reference.md).    



### Obtain Cluster JSON Template from the UI

The simplest way to obtain a valid JSON template for your cluster is to get it from the Cloudbreak UI. You can do this in two ways:

**From Create Cluster**

Once you've provided all the cluster parameters, on the last page of the create cluster wizard, click **Show CLI Command** to obtain the JSON template:

<a href="../images/cli-json-create-cluster2.png" target="_blank" title="click to enlarge"><img src="../images/cli-json-create-cluster2.png" width="650" title="Cloudbreak web UI"></a>    

Click **Copy the JSON** to copy the content and then use a text editor to edit and save it. 


**From Cluster Details**

You can obtain the JSON template for a cluster from the cluster details page by selecting **Actions** > **Show CLI Command**. This option is available for all clusters that have been initiated, so the cluster does not need to be in the running state to obtain this information. In fact, this option is useful when troubleshooting cluster failures.  

<a href="../images/cli-json-details1.png" target="_blank" title="click to enlarge"><img src="../images/cli-json-details1.png" width="650" title="Cloudbreak web UI"></a>   

Click **Copy the JSON** to copy the content and then use a text editor to edit and save it. 

<a href="../images/cli-json-details2.png" target="_blank" title="click to enlarge"><img src="../images/cli-json-details2.png" width="650" title="Cloudbreak web UI"></a> 


### Obtain CLI Command from the UI

Cloudbreak web UI includes an option in the UI which allows you to generate the  `create` command for resources such as credentials, blueprints, clusters, and recipes. This option is available when creating a resource and for existing resources, from the resource details page.   

**From Create Resource**

When creating a resource (credential, blueprint, cluster, or recipe), provide all information and then click **Show CLI Command**. The UI will display the `create` CLI command for the resource.

**From Resource Details**

Navigate to credential, blueprint, cluster, or recipe details and  click **Show CLI Command**. The UI will display the `create` CLI command for the resource.


### Get Help

To get CLI help, you can add help to the end of a command. The following will list help for the CLI at the top-level:

<pre><small>cb --help</small></pre>

or 

<pre><small>cb --h</small></pre>

The following will list help for the create-cluster command, including its command options and global options:

<pre><small>cb cluster --help</small></pre>

or

<pre><small>cb cluster --h</small></pre> 



<div class="next">
<a href="../cli-reference/index.html">Next: CLI Reference</a>
</div>