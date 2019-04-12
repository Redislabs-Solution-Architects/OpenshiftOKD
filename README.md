# OpenshiftOKD
Openshift Install
This is to install Openshift with 1 Master and 3 Nodes on AWS
Environment Preparation:
  1. Define Tags
  2. Create an IAM Rules
  3. SSH Access from the Master Node to all Nodes
  4. DNS Configuration
  5. Firewall Rules
Openshift Install Preparation:
  1. Preparing the Inventory File
  2. Preparing the prepare.yaml file
Installing Pre-Requisite Software:
  1. Install epal-release --- yum install epel-release
  2. Install git --- yum -y install git
  3. Install pip --- yum -y install python-pip
  4. Install Ansible --- pip install ansible==2.6.5
Clone the Openshift-ansible Repository
  1. git clone https://github.com/openshift/openshift-ansible
  2. cd openshift-ansible
  3. git checkout release-3.11
 
