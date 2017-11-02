## Install Cloudbreak CLI  


The Cloudbreak Command Line Interface (CLI) is a tool to help you manage your Cloudbreak cluster instances. This tool can be used to interact with Cloudbreak for automating cluster creation, management, monitoring, and termination. 

The CLI is available for Linux, Mac OS X, and Windows. 


### Install 

After you have launched Cloudbreak, the CLI is available for download from that Cloudbreak instance.

**Steps**

1. Browse to your Cloudbreak instance and log in to the Cloubdreak web UI.  
2. Select **Download CLI** from the navigation pane. 
3. Select your operating system. The CLI is available for Linux, Mac OS X, and Windows.  
4. Download the selected bundle to your local machine.  
5. Extract the bundle.  
6. You can optionally add `cb` to your system path.
7. Run the executable to verify the CLI: 

    <pre>cb --version</pre>


### Configure

Once you have installed the CLI, you need to configure the CLI to work with Cloudbreak.

**Steps**

1. Use the `cb configure` command to set up the CLI configuration file. The configuration options are:  
    * **--server** server address [$CB_SERVER_ADDRESS]  
    * **--username** user name (e-mail address) [$CB_USER_NAME]  
    * **--password** password [$CB_PASSWORD]  
   
    The password configuration is optional. If you do not provide the password, no password is stored in the CLI configuration file. Therefore, you will need to provide the password with each command you execute or via an environment variable.
    
    For example:
    
    <pre>cb configure --server https://ec2-11-111-111-11.compute-1.amazonaws.com --username admin@hortonworks.com</pre>

2. The CLI configuration file will be saved at `~/.cb/config`. The content will look similar to the following:

    <pre>default:
  username: admin@hortonworks.com
  server: https://ec2-11-111-111-11.compute-1.amazonaws.com</pre>


3. Run any command to verify that you can connect to the Cloudbreak instance via CLI. For example:

    <pre>cb list-clusters</pre>  


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

If you are using multiple profiles for multiple environments, you can configure them using the `cb configure` command and passing `--profile value` parameter. After running the command, the configuration will be added as a new entry to the `config` file. For example:

<pre>default
  username: admin@hortonworks.com
  server: https://192.167.65.4
staging
  username: test@hortonworks.com
  server: https://192.176.122.1  
</pre>

    
[comment]: <> (Is this correct? Need to clarify how this works if you have multiple profiles. Would you add multiple entries to the config file manually, or can you do it using the configure command? Is the entry name supposed to match profile name?)  


#### Configure Default Output

By default, JSON format is used in command output. For example, if you run `cb list-clusters` without specifying output type, the output will be JSON. If you would like to change default output, add it to the config file. For example:

<pre>default
  username: admin@hortonworks.com
  server: https://192.167.65.4
  output: table</pre>


### Getting Help

To get CLI help, you can add help to the end of a command. The following will list help for the CLI at the top-level:

<pre>cb help</pre>

The following will list help for the create-cluster command, including its command options and global options:

<pre>cb create-cluster help</pre>


### Debugging

To use debugging mode, pass the `--debug` option. 


### Checking CLI Version

To check CLI version, use `cb --version`.


<div class="next">
<a href="../cli-reference/index.html">Next: CLI Reference</a>
</div>