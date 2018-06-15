## Installing Cloudbreak CLI  


The Cloudbreak Command Line Interface (CLI) is a tool to help you manage your Cloudbreak cluster instances. This tool can be used to interact with Cloudbreak for automating cluster creation, management, monitoring, and termination. 

The CLI is available for Linux, Mac OS X, and Windows. 


### Install the CLI

After you have launched Cloudbreak, the CLI is available for download from that Cloudbreak instance.

**Steps**

1. Browse to your Cloudbreak instance and log in to the Cloudbreak web UI.  
2. Select **Download CLI** from the navigation pane. 
3. Select your operating system. The CLI is available for Linux, Mac OS X, and Windows.   
4. Download the selected bundle to your local machine.  
5. Extract the bundle.  
6. You can optionally add `cb` to your system path.
7. Run the executable to verify the CLI: 

    <pre><small>cb --version</small></pre>


### Configure the CLI 

Once you have installed the CLI, you need to configure the CLI to work with Cloudbreak.

**Steps**

1. Use the `cb configure` command to set up the CLI configuration file. The configuration options are:  
    * **--server** server address [$CB_SERVER_ADDRESS]  
    * **--username** user name (e-mail address) [$CB_USER_NAME]  
    * **--password** password [$CB_PASSWORD]  
   
    The password configuration is optional. If you do not provide the password, no password is stored in the CLI configuration file. Therefore, you will need to provide the password with each command you execute or via an environment variable.
    
    For example:
    
    <pre><small>cb configure --server https://ec2-11-111-111-11.compute-1.amazonaws.com --username admin@hortonworks.com</small></pre>

2. The CLI configuration file will be saved at `~/.cb/config`. The content will look similar to the following:

    <pre><small>default:
  username: admin@hortonworks.com
  server: https://ec2-11-111-111-11.compute-1.amazonaws.com</small></pre>


3. Run any command to verify that you can connect to the Cloudbreak instance via CLI. For example:

    <pre><small>cb cluster list</small></pre>  


<div class="note">
    <p class="first admonition-title">Configuration precedence</p>
    <p class="last">
    The CLI can look for configuration options from different locations. You can optionally
    pass the configuration options on each command or from environment variables. The following
    order is used for the CLI to look for configuration options: <strong>Command line</strong>, <strong>Environment variables</strong>
    and the <strong>Configuration file</strong>.
    </p>
</div>

  
#### Add multiple configurations  

If you are using multiple profiles for multiple environments, you can configure them using the `cb configure` command and passing the name of your environment-specific profile file using the `--profile` parameter. After running the command, the configuration will be added as a new entry to the `config` file. For example, running the following command `cb configure --server https://192.167.65.4 --username test@hortonworks.com --profile staging` will add the "staging" entry:

<pre><small>default:
  username: admin@hortonworks.com
  server: https://192.167.65.4
staging:
  username: test@hortonworks.com
  server: https://192.167.65.4  
</small></pre>

For example:

<pre><small>#cb configure --server https://192.167.65.4 --username test@hortonworks.com --profile staging
INFO:  [writeConfigToFile] dir already exists: /Users/rkovacs/.cb
INFO:  [writeConfigToFile] writing credentials to file: /Users/rkovacs/.cb/config
# cat /Users/rkovacs/.cb/config
default:
  username: admin@example.com
  server: https://192.167.65.4
  output: table
staging:
  username: test@hortonworks.com
  server: https://192.167.65.4</small></pre>
 


#### Configure default output

By default, JSON format is used in command output. For example, if you run `cb list-clusters` without specifying output type, the output will be JSON. If you would like to change default output, add it to the config file. For example:

<pre><small>default:
  username: admin@hortonworks.com
  server: https://192.167.65.4
  output: table</small></pre>


#### Configure CLI autocomplete 

The CLI includes an autocomplete option. Before you can use this option, you must download and source one of the following files:

* For **bash**: bash_autocomplete  
* For **zsh**: zsh_autocomplete  

These two files are located in [https://github.com/hortonworks/cb-cli/tree/master/autocomplete/](https://github.com/hortonworks/cb-cli/tree/master/autocomplete/)  

Once you've sourced the file, type the CLI commands as usual and use the **Tab** key to access the autocomplete feature.  


## Getting started with the CLI   

### Get started with the CLI 

After [installing](https://hortonworks.github.io/cloudbreak-documentation/latest/) and [configuring](https://hortonworks.github.io/cloudbreak-documentation/latest/) the CLI, you can use it to perform the same tasks as are available in the Cloudbreak UI: create and manage clusters, credentials, blueprints, and recipes.

**Steps**

1. Before you start using the CLI, familiarize yourself with Cloudbreak concepts and the Cloudbreak web UI. 

2. If you haven't already, create at least one Cloudbreak credential by using Cloudbreak UI or the [credential create](https://hortonworks.github.io/cloudbreak-documentation/latest/) command. 

3. To create a cluster, you must first generate a JSON skeleton. Although it is possible to generate it by using the [cluster generate-template](https://hortonworks.github.io/cloudbreak-documentation/latest/) command, it is easiest to obtain it from the Cloudbreak UI, as described in [Obtain cluster JSON template from the UI](https://hortonworks.github.io/cloudbreak-documentation/latest/).

4. Save the template in the JSON format and edit it if needed.

5. Once your JSON file is ready, you can use it to create a cluster via the [cluster create](https://hortonworks.github.io/cloudbreak-documentation/latest/) command.

6. Once your cluster is running, use can use the CLI to manage and monitor your cluster. For a full list of commands, refer to [CLI reference](https://hortonworks.github.io/cloudbreak-documentation/latest/).    



### Obtain cluster JSON template from the UI

The simplest way to obtain a valid JSON template for your cluster is to get it from the Cloudbreak UI. You can do this in two ways:

**From create cluster**

Once you've provided all the cluster parameters, on the last page of the create cluster wizard, click **Show CLI Command** to obtain the JSON template:

<img src="../images/cb_cli-json-create-cluster2.png" width="650" title="Cloudbreak web UI">    

Click **Copy the JSON** to copy the content and then use a text editor to edit and save it. 


**From cluster details**

You can obtain the JSON template for a cluster from the cluster details page by selecting **Actions** > **Show CLI Command**. This option is available for all clusters that have been initiated, so the cluster does not need to be in the running state to obtain this information. In fact, this option is useful when troubleshooting cluster failures.  

<img src="../images/cb_cli-json-details1.png" width="650" title="Cloudbreak web UI"> 

Click **Copy the JSON** to copy the content and then use a text editor to edit and save it. 

<img src="../images/cb_cli-json-details2.png" width="650" title="Cloudbreak web UI">


### Obtain CLI command from the UI

Cloudbreak web UI includes an option in the UI which allows you to generate the  `create` command for resources such as credentials, blueprints, clusters, and recipes. This option is available when creating a resource and for existing resources, from the resource details page.   

**From Create Resource**

When creating a resource (credential, blueprint, cluster, or recipe), provide all information and then click **Show CLI Command**. The UI will display the `create` CLI command for the resource.

**From Resource Details**

Navigate to credential, blueprint, cluster, or recipe details and  click **Show CLI Command**. The UI will display the `create` CLI command for the resource.


### Get help

To get CLI help, you can add help to the end of a command. The following will list help for the CLI at the top-level:

<pre><small>cb --help</small></pre>

or 

<pre><small>cb --h</small></pre>

The following will list help for the create-cluster command, including its command options and global options:

<pre><small>cb cluster --help</small></pre>

or

<pre><small>cb cluster --h</small></pre> 


