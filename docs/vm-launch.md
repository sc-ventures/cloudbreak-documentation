## Installing Cloudbreak on your own VM

This is a production deployment option. Select this option if you have custom requirements. Otherwise, you can launch Cloudbreak from a template:

* [Launch on AWS](aws-launch.md)  
* [Launch on Azure](azure-launch.md)  
* [Launch on GCP](gcp-launch.md)  
* [Launch on OpenStack](os-launch.md)   


### System requirements

To launch the Cloudbreak deployer and install the Cloudbreak application, your system must meet the following requirements:

* Minimum VM requirements: 16GB RAM, 40GB disk, 4 cores
* Supported operating systems: RHEL, CentOS, and Oracle Linux 7 (64-bit)
* Docker 1.9.1 must be installed

> You can install Cloudbreak on Mac OS X for evaluation purposes only. Mac OS X is not supported for a production deployment of Cloudbreak.


### Prerequisites

To launch the Cloudbreak deployer and install the Cloudbreak application, you must first meet the following prerequisites:

#### Ports

Ports 22 (SSH), 80 (HTTPS), and 443 (HTTPS) must be open.

#### Root access

Every command must be executed as root. In order to get root privileges execute:

<pre>sudo -i</pre>

#### System updates

Ensure that your system is up-to-date by executing:

<pre>yum -y update</pre>

Reboot it if necessary.

#### Iptables

1. Install iptables-services:

    <pre>yum -y install net-tools ntp wget lsof unzip tar iptables-services
systemctl enable ntpd && systemctl start ntpd
systemctl disable firewalld && systemctl stop firewalld</pre>

    > Without iptables-services installed the `iptables save` command will not be available.

2. Configure permissive iptables on your machine:

    <pre>iptables --flush INPUT && \
iptables --flush FORWARD && \
service iptables save</pre>

3. Disable SELINUX:
    
    <pre>sed -i --follow-symlinks 's/^SELINUX=.*/SELINUX=disabled/g' /etc/sysconfig/selinux</pre>

4. Create Docker repo:

    <pre>cat > /etc/yum.repos.d/docker.repo <<"EOF"
[dockerrepo]
name=Docker Repository
baseurl=https://yum.dockerproject.org/repo/main/centos/7
enabled=1
gpgcheck=1
gpgkey=https://yum.dockerproject.org/gpg
EOF</pre>

5. Install Docker service:

    <pre>yum install -y docker-engine-1.9.1 docker-engine-selinux-1.9.1
systemctl start docker
systemctl enable docker</pre>


#### Cloud Provider Prerequisites 

Additionally, review the following cloud provider prerequisites:

* [Prerequisites on AWS](aws-launch.md#meet-the-prerequisites)
* [Prerequisites on Azure](azure-launch.md#meet-the-prerequisites)
* [Prerequisites on GCP](gcp-launch.md#meet-the-prerequisites)
* [Prerequisites on OpenStack](os-launch.md#meet-the-prerequisites)


### Install Cloudbreak on your own VM

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

    <pre>cbd pull
cbd start</pre>

    This will start the Docker containers and initialize the application. The first time you start the Cloudbreak app, the process will take longer than usual due to the download of all the necessary docker images.

5. Next, check Cloudbreak application logs:

    <pre>cbd logs cloudbreak</pre>

    You should see a message like this in the log: `Started CloudbreakApplication in 36.823 seconds.` Cloudbreak normally takes less than a minute to start.

### Configure external Cloudbreak database

By default, Cloudbreak uses an embedded PostgreSQL database to persist data related to Cloudbreak configuration, setup, and so on. For a production Cloudbreak deployment, we suggest that you [configure an external database](cb-db.md).


### Access Cloudbreak web UI

Log in to the Cloudbreak UI using the following steps.

**Steps** 

1. You can log into the Cloudbreak application at `https://IP_Address`. For example `https://34.212.141.253`. You can obtain the VM's IP address from your cloud provider console.

{!docs/common/launch-access-ui.md!} 
    
4. Log in to the Cloudbreak web UI using the credentials that you configured in your `Profile` file:

    * The username is the `UAA_DEFAULT_USER_EMAIL`     
    * The password is the `UAA_DEFAULT_USER_PW` 

5. Upon a successful login, you are redirected to the dashboard:

    <a href="../images/cb_cb-ui1.png" target="_blank" title="click to enlarge"><img src="../images/cb_cb-ui1.png" width="650" title="Cloudbreak web UI"></a>  

### Next steps

Before you can create clusters, you must configure a Cloudbreak credential. Refer to the steps for your cloud platform:

* [Create a Cloudbreak credential on AWS](aws-launch.md#create-cloudbreak-credential)  
* [Create a Cloudbreak credential on Azure](azure-launch.md#create-cloudbreak-credential)  
* [Create a Cloudbreak credential on GCP](gcp-launch.md#create-cloudbreak-credential)  
* [Create a Cloudbreak credential on OpenStack](os-launch.md#create-cloudbreak-credential)  
