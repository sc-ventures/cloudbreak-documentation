
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

#### Iptables

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

3. Disable SELINUX:
    
    <pre>sed -i --follow-symlinks 's/^SELINUX=.*/SELINUX=disabled/g' /etc/sysconfig/selinux</pre>
    
    
#### Docker 

Perform these steps to install Docker.

**Steps**    

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


