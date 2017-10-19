## Cloudbreak Shell  

>>>>TO-DO: If we have new CLI, then this info will need to be updated.

Cloudbreak Shell is an interactive command line tool which:

* Supports all functionality available through the REST API and Cloudbreak web UI
* Makes possible complete automation of management task via scripts
* Includes context-aware commands
* Allows tab completion
* Supports required and optional parameters
* Includes a hint command to guide you

### Install and Start Cloudbreak Shell 

There are three ways to install and run Cloudbreak Shell:

* From the Cloudbreak VM (Recommended) 
* From the Docker Image  
* Build From Source  

> The latter two methods run the CLI on your local machine. The first method runs the CLI on the VM but provides an option to run the commands on your local machine. 

#### From the Cloudbreak VM

The easiest way to install and start Cloudbreak Shell is from the VM on which you have deployed Cloudbreak. On the VM, navigate to the `/var/lib/cloudbreak-deployment/` directory and run:

<pre>cbd util cloudbreak-shell</pre>

If you would like to use Cloudbreak Shell from your local machine, follow these steps.

**Steps**

1. Execute `cbd util cloudbreak-shell-remote` on the VM where Cloudbreak is running.

2. Copy the output of the above command. 

3. Paste and execute the output on your local machine.

#### From the Docker Image

You can find the docker image and its documentation [here](https://github.com/hortonworks/docker-cloudbreak-shell).

>>>>TO-DO: Add from the github doc? Is this still valid? 

#### Build From Source

>>>>TO-DO: Add from the old doc. Is this still valid? 

>>>>TO-DO: Include documentation from: (the repository url changed because the project is now under hwx)

* [Cloudbreak docs](http://sequenceiq.com/cloudbreak-docs/latest/shell/) 
* [Cloudbreak Shell](https://github.com/sequenceiq/cloudbreak/tree/master/shell) 
* Provisioning clusters via Cloudbreak Shell 
