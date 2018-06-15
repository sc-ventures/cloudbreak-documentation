 
## Meet minimum system requirements

Before launching Cloudbreak on your OpenStack, make sure that your OpenStack deployment fulfills the following requirements.

### Supported Linux distributions

The following versions of the [Red Hat distribution of OpenStack](https://www.rdoproject.org/) (RDO) are supported:

* Juno
* Kilo
* Liberty
* Mitaka

### Standard modules

Cloudbreak requires that the following standard modules are installed and configured on OpenStack:

* Keystone V2 or Keystone V3
* Neutron (Self-service and provider networking)
* Nova (KVM or Xen hypervisor)
* Glance
* Cinder (Optional)
* Heat (Optional but highly recommended, since provisioning through native API calls will be deprecated in the future)


## Meet the prerequisites

Before launching Cloudbreak on OpenStack, you must meet the following prerequisites.

### SSH key pair

[Generate a new SSH key pair](https://hortonworks.github.io/cloudbreak-documentation/latest/) or use an existing SSH key pair to your OpenStack account. You will be required to select it when launching the VM.

### Virtual network

You must have a virtual network configured on your cloud provider.

### Security group

Ports 22 (SSH), 80 (HTTPS), and 443 (HTTPS) must be open on the security group.




## Launching Cloudbreak on OpenStack

These steps describe how to launch Cloudbreak on OpenStack. This is the only deployment options available on OpenStack.  
Before launching Cloudbreak on OpenStack, review and meet the prerequisites. Next, follow the steps below. 


### VM requirements

To launch the Cloudbreak deployer and install the Cloudbreak application, you must have an existing VM. 

#### System requirements  

Your system must meet the following requirements:

* Minimum VM requirements: 16GB RAM, 40GB disk, 4 cores
* Supported operating systems: RHEL, CentOS, and Oracle Linux 7 (64-bit)

> You can install Cloudbreak on Mac OS X for evaluation purposes only. Mac OS X is not supported for a production deployment of Cloudbreak.


#### Root access

Every command must be executed as root. In order to get root privileges execute:

<pre>sudo -i</pre>


#### System updates

Ensure that your system is up-to-date by executing:

<pre>yum -y update</pre>

Reboot it if necessary.


#### Install iptables

Perform these steps to install and configure iptables.

**Steps** 

1. Install iptables-services:

    <pre>yum -y install net-tools ntp wget lsof unzip tar iptables-services
systemctl enable ntpd && systemctl start ntpd
systemctl disable firewalld && systemctl stop firewalld</pre>

    > Without iptables-services installed the `iptables save` command will not be available.

2. Configure permissive iptables on your machine:

    <pre>iptables --flush INPUT && \
iptables --flush FORWARD && \
service iptables save</pre>


#### Disable SELINUX

Perform these steps to disable SELINUX.

**Steps** 

1. Disable SELINUX:
    
    <pre>setenforce 0
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' 
/etc/selinux/config</pre>

2. Run the following command to ensure that SELinux is not turned on afterwards: 

    <pre>getenforce</pre>
    
3. The command should return "Disabled".     

    
#### Install Docker 

Perform these steps to install Docker. The minimum Docker version is 1.13.1. If you are using an older image that comes with an older Docker version, upgrade Docker to 1.13.1 or newer. 

**Steps**    

1. Install Docker service:

    <pre>yum install -y docker
systemctl start docker
systemctl enable docker</pre>

3. Check the Docker Logging Driver configuration:

    <pre>docker info | grep "Logging Driver"</pre>
    
4. If it is set to `Logging Driver: journald`, you must  set it to "json-file" instead. To do that:

    1. Open the `docker` file for editing:
    
        <pre>vi /etc/sysconfig/docker</pre>  
        
    2. Edit the following part of the file so that it looks like below (showing `log-driver=json-file`):

        <pre># Modify these options if you want to change the way the docker daemon runs
OPTIONS='--selinux-enabled --log-driver=json-file --signature-verification=false'</pre>     

    3. Restart Docker:

        <pre>systemctl restart docker
systemctl status docker</pre>



### Install Cloudbreak on a VM

Install Cloudbreak using the following steps.


**Steps**

1. Install the Cloudbreak deployer and unzip the platform-specific single binary to your PATH. For example:

    <pre>yum -y install unzip tar
curl -Ls public-repo-1.hortonworks.com/HDP/cloudbreak/cloudbreak-deployer_2.7.0_$(uname)_x86_64.tgz | sudo tar -xz -C /bin cbd
cbd --version</pre>


    Once the Cloudbreak deployer is installed, you can set up the Cloudbreak application.

2. Create a Cloudbreak deployment directory and navigate to it:

    <pre>mkdir cloudbreak-deployment
cd cloudbreak-deployment</pre>

3. In the directory, create a file called `Profile` with the following content:

    <pre>export UAA_DEFAULT_SECRET=MY-SECRET
export UAA_DEFAULT_USER_PW=MY-PASSWORD
export UAA_DEFAULT_USER_EMAIL=MY-EMAIL
export PUBLIC_IP=MY_VM_IP</pre>

    For example:

    <pre>export UAA_DEFAULT_SECRET=MySecret123
export UAA_DEFAULT_USER_PW=MySecurePassword123
export UAA_DEFAULT_USER_EMAIL=dbialek@hortonworks.com
export PUBLIC_IP=172.26.231.100</pre>

    You will need to provide the email and password when logging in to the Cloudbreak web UI and when using the Cloudbreak CLI. The secret will be used by Cloudbreak for authentication.
    
    You should set the CLOUDBREAK_SMTP_SENDER_USERNAME variable to the username you use to authenticate to your SMTP server. You should set the CLOUDBREAK_SMTP_SENDER_PASSWORD variable to the password you use to authenticate to your SMTP server.

4. Generate configurations by executing:

    <pre>rm *.yml
cbd generate</pre>   

    The cbd start command includes the cbd generate command which applies the following steps:

    * Creates the `docker-compose.yml` file, which describes the configuration of all the Docker containers required for the Cloudbreak deployment.  
    * Creates the `uaa.yml` file, which holds the configuration of the identity server used to authenticate users with Cloudbreak.   

5. Start the Cloudbreak application by using the following commands:

    <pre>cbd pull-parallel
cbd start</pre>

    This will start the Docker containers and initialize the application. The first time you start the Cloudbreak app, the process will take longer than usual due to the download of all the necessary docker images.
     

5. Next, check Cloudbreak application logs:

    <pre>cbd logs cloudbreak</pre>

    You should see a message like this in the log: `Started CloudbreakApplication in 36.823 seconds.` Cloudbreak normally takes less than a minute to start.  


### Access Cloudbreak web UI

Log in to the Cloudbreak UI using the following steps.

**Steps** 

1. You can log into the Cloudbreak application at `https://IP_Address`. For example `https://34.212.141.253`. You may use `cbd start` to obtain the login information. Alternatively, you can obtain the VM's IP address from your cloud provider console. 


2. Confirm the security exception to proceed to the Cloudbreak web UI.

    The first time you access Cloudbreak UI, Cloudbreak will automatically generate a self-signed certificate, due to which your browser will warn you about an untrusted connection and will ask you to confirm a security exception.
        
    | Browser | 	Steps |
|---|---|
| Firefox |Click **Advanced** > Click **Add Exception...** > Click **Confirm Security Exception** |
| Safari | Click **Continue** |
| Chrome | Click **Advanced** > Click **Proceed...** |

3. The login page is displayed:

    <img src="../images/cb_cb-ui.png" width="650" title="Cloudbreak web UI">  
    
    
4. Log in to the Cloudbreak web UI using the credentials that you configured in your `Profile` file:

    * The username is the `UAA_DEFAULT_USER_EMAIL`     
    * The password is the `UAA_DEFAULT_USER_PW` 

5. Upon a successful login, you are redirected to the dashboard:

    <img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI">  
    

### Configure external Cloudbreak database

By default, Cloudbreak uses an embedded PostgreSQL database to persist data related to Cloudbreak configuration, setup, and so on. For a production Cloudbreak deployment, you must [configure an external database](https://hortonworks.github.io/cloudbreak-documentation/latest/).





### Configure a self-signed certificate

If your OpenStack is secured with a self-signed certificate, you need to import that certificate into Cloudbreak, or else Cloudbreak won't be able to communicate with your OpenStack.

To import the certificate, place the certificate file in the `/certs/trusted/` directory, follow these steps.

**Steps**

1. Navigate to the `certs` directory (automatically generated).
2. Create the `trusted` directory.
3. Copy the certificate to the `trusted` directory.

Cloudbreak will automatically pick up the certificate and import it into its trust store upon start.


### Create Cloudbreak credential

Cloudbreak works by connecting your OpenStack account through this credential, and then uses it to create resources on your behalf. Before you can start provisioning cluster using Cloudbreak, you must create a [Cloudbreak credential](https://hortonworks.github.io/cloudbreak-documentation/latest/).

**Steps**

1. In the Cloudbreak web UI, select **Credentials** from the navigation pane.

2. Click **Create Credential**.

3. Under **Cloud provider**, select "Google Cloud Platform".

    <img src="../images/cb_cb-os-cred.png" width="650" title="Cloudbreak web UI">

3. Select the keystone version.

4. Provide the  following information:

    For Keystone v2:

    | Parameter | Description |
|---|---|
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| User | Enter your OpenStack user name. |
| Password | Enter your OpenStack password. |
| Tenant Name | Enter the OpenStack tenant name. |
| Endpoint | Enter the OpenStack endpoint. |
| API Facing | (Optional) Select *public*, *private*, or *internal*. |

    For Keystone v3:

    | Parameter | Description |
|---|---|
| Keystone scope | Select the scope: default, domain, or project. |
| Name | Enter a name for your credential. |
| Description | (Optional) Enter a description. |
| User | Enter your OpenStack user name. |
| Password | Enter your OpenStack password. |
| User Domain | Enter your OpenStack user domain. |
| Endpoint | Enter the OpenStack endpoint. |
| API Facing | (Optional) Select *public*, *private*, or *internal*. |

[comment]: <> (Not sure what these params do: Keystone scope, User Domain)

4. Click **Create**.

5. Your credential should now be displayed in the **Credentials** pane.

    Congratulations! You have successfully launched Cloudbreak and created a Cloudbreak credential. Now you can use Cloudbreak to [create clusters](https://hortonworks.github.io/cloudbreak-documentation/latest/).




