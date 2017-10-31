## Install Cloudbreak CLI  


The Cloudbreak Command Line Interface (CLI) is a tool to help you manage your Cloudbreak cluster instances. This tool can be used to interact with Cloudbreak for automating cluster creation, management, monitoring, and termination. 

The CLI is available for Linux, Mac OS X, and Windows. 


### Install 

After you have launched Cloudbreak, the CLI is available for download from that Cloudbreak instance.

**Steps**

1. Browse to your Cloudbreak instance and log in to the Cloubdreak web UI.  
2. Select **Download CLI** from the menu. The CLI is available for Linux, Mac OS X, and Windows.  
3. Download the selected bundle to your local machine.  
4. Extract the bundle.  
5. You can optionally add `cb` to your system path.
6. Run the executable to verify the CLI: 

    <pre>cb --version</pre>


### Configure

Once you have installed the CLI, you need to configure the CLI to work with Cloudbreak.

**Steps**

1. Use the `cb configure` command to set up the CLI configuration file. The configuration options are:  
    * **--server** server address [$CB_SERVER_ADDRESS]  
    * **--username** user name (e-mail address) [$CB_USER_NAME]  
    * **--password** password [$CB_PASSWORD]  
   
    The password configuration is optional. If you do not provide the password, no password is stored in the CLI configuration file. Therefore, you will need to provide the password with each command you execute or via an environment variable.

2. The CLI configuration file will be saved at `~/.cb/config`.

[comment]: <> (Add example content of the config file)

<div class="note">
    <p class="first admonition-title">Configuration Precedence</p>
    <p class="last">
    The CLI can look for configuration options from different locations. You can optionally
    pass the configuration options on each command or from environment variables. The following
    order is used for the CLI to look for configuration options: <strong>Command Line</strong>, <strong>Environment Variables</strong>
    and the <strong>Configuration File</strong>.
    </p>
</div>

[comment]: <> (Need to clarify how this works if you have multiple profiles. Would you add multiple entries to the config file manually? Is the entry name supposed to match profile name?)


### Get Help

To get CLI help, you can add help to the end of a command. The following will list help for the CLI at the top-level:

<pre>cb help</pre>

The following will list help for the create-cluster command, including its command options and global options:

<pre>cb create-cluster help</pre>


### Debug

To use debugging mode, pass the `--debug` option. 


### Check CLI Version

To check CLI version, use `cb --version`.


### Command Structure

The CLI command can contain multiple parts. The first part is a set of global options. The next part is the command. The next part is a set of command options and arguments which could include sub-commands.

<pre>cb [global options] command [command options] [arguments...]</pre>


### Command Output

You can control the output from the CLI using the --output argument. The possible output formats include:

* Formatted table (table)
* JSON (json)

For example:

<pre>cb list-clusters --output json</pre>

<pre>cb list-clusters --output table</pre>

[comment]: <> (Add actual output)

[comment]: <> (I think you can also add default output to the config file?)

[comment]: <> (Example:)
[comment]: <> (default:)
[comment]: <> (  username: admin@example.com)
[comment]: <> (  password: MySecurePass123)
[comment]: <> (  server: https://192.167.65.4)
[comment]: <> (  output: table))


<div class="next">
<a href="../cli-using/index.html">Next:Using Cloudbreak CLI</a>
</div>