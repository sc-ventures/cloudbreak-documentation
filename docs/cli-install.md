## Installing Cloudbreak CLI  


The Cloudbreak Command Line Interface (CLI) is a tool to help you manage your Cloudbreak cluster instances. This tool can be used to interact with Cloudbreak for automating cluster creation, management, monitoring, and termination. 

The CLI is available for Linux, Mac OS X, and Windows. 


### Install the CLI

After you have launched Cloudbreak, the CLI is available for download from that Cloudbreak instance.

**Steps**

1. Browse to your Cloudbreak instance and log in to the Cloubdreak web UI.  
2. Select **Download CLI** from the navigation pane. 
3. Select your operating system. The CLI is available for Linux, Mac OS X, and Windows:  
    <a href="../images/cb-cli.png" target="_blank" title="click to enlarge"><img src="../images/cb-cli.png" width="650" title="Cloudbreak web UI"></a>  
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

    <pre><small>cb list-clusters</small></pre>  


<div class="note">
    <p class="first admonition-title">Configuration Precedence</p>
    <p class="last">
    The CLI can look for configuration options from different locations. You can optionally
    pass the configuration options on each command or from environment variables. The following
    order is used for the CLI to look for configuration options: <strong>Command Line</strong>, <strong>Environment Variables</strong>
    and the <strong>Configuration File</strong>.
    </p>
</div>

  
#### Add Multiple Configurations  

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
 


#### Configure Default Output

By default, JSON format is used in command output. For example, if you run `cb list-clusters` without specifying output type, the output will be JSON. If you would like to change default output, add it to the config file. For example:

<pre><small>default:
  username: admin@hortonworks.com
  server: https://192.167.65.4
  output: table</small></pre>




<div class="next">
<a href="../cli-get-started/index.html">Next: Get Started with CLI</a>
</div>