# OpenshiftOKD
Openshift Install

This is to install Openshift with 1 Master and 3 Nodes on AWS

Environment Preparation:

  1. Define Tags for all instances
    
        key: kubernetes.io/cluster/ocp 
        value: Value=(owned|shared)
      
  2. Create an IAM Rules
  
       Create an IAM role with administrator access and assign it as a tag for all instances
  3. DNS Configuration
  
    a. define the DNS configuration for all nodes
    
        Example Configuration:
          *.okdw1.demo-rlec.redislabs.com     CNAME   master.okdw1.demo-rlec.redislabs.com
          okdw1.demo-rlec.redislabs.com       CNAME   master.okdw1.demo-rlec.redislabs.om
          master.okdw1.demo-rlec.redislabs.com  A     <IP Address>
          node1.okdw1.demo-rlec.redislabs.com   A     <IP Address>
          node2.okdw1.demo-rlec.redislabs.com   A     <IP Address>
          node3.okdw1.demo-rlec.redislabs.com   A     <IP Address>
  
  4. SSH Access from the Master Node to all Nodes
  
      a. Ensure the ssh key is available on the master node 
      b. test ssh -i <key> centos@<fqdn of node>
         
         Example:
          ssh -i ssh_key.pem centos@master.okdw1.demo-rlec.redislabs.com
          ssh -i ssh_key.pem centos@node1.okdw1.demo-rlec.redislabs.com
          ssh -i ssh_key.pem centos@node2.okdw1.demo-rlec.redislabs.com
          ssh -i ssh_key.pem centos@node3.okdw1.demo-rlec.redislabs.com
  
  5. Firewall Rules
    a. ensure all nodes are accessible using the external IP address (all traffic)
    
      a. all traffic to and from all external IP of the nodes
      b. all internal traffic for the security group
      c. DNS ports
  
Openshift Install Preparation:
  
  1. Preparing the Inventory File
      
  2. Preparing the prepare.yaml file
  
Installing Pre-Requisite Software:
  
  1. Install epal-release 
  
      yum install epel-release
  
  2. Install git  
  
      yum -y install git
  
  3. Install pip  
  
      yum -y install python-pip
  
  4. Install Ansible  
  
        pip install ansible==2.6.5
        
Make a directory 
  
        cd /
        mkdir okd
        cd okd

Clone the Openshift-ansible Repository
  
      git clone https://github.com/openshift/openshift-ansible
      cd openshift-ansible
      git checkout release-3.11
 
 Run the prepare.yaml script
  
      cd ..
      ansible-playbook prepare.yaml -i inventory --key-file ssh_key.pem
 
 Run the pre-requisites.yaml script
  
      ansible-playbook openshift-ansible/playbooks/prerequisites.yml -i inventory --key-file ssh_key.pem
 
 Deploy the cluster
  
      ansible-playbook openshift-ansible/playbooks/deploy_cluster.yml -i inventory --key-file ssh_key.pem
     (Note -- you may have an authentication failure and the deployment may stop then. Please restart the deployment and it         should finish successfully)
