**WHAT IS ANSIBLE**

"Ansible is a powerful configuration management tool designed to facilitate the installation, maintenance, management, and updating of packages on servers through automation. It supports YAML, a human-readable programming language, for defining tasks and configurations. Utilizing a push-based mechanism, Ansible allows users to define tasks on a central server, which are then automatically configured on the target servers. One of its key advantages is its agentless architecture, eliminating the need to install Ansible on the nodes or hosted servers, thereby simplifying management and reducing overhead."


**ANSIBLE CONFIGURATION**:

providing complete description how to configure ansible master and connecting with nodes.

- I am going to launch 3 EC2 instances on AWS. I configure one EC2 instance as a master and others will be configured as a nodes. I will use ansible master to configure
  the nodes. There are following steps need to be proceed.

 - **STEP 1:** Firstly, I have to download the ansible package for the master server by using this following command.
 - **wget https:// dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm**
 - varify the download package by using = ls. If it is successfully downloaded then I need to install the package on the master server.
 - **yum install -y epel/epel-release-latest-7.noarch.rpm**
 - then need to update the ansible master server so that allows to install all the important dependencies for the installing the ansible successfully on the master server
 - **yum update -y**
 - afterthat we can install ansible on the master server by following this command
 - **yum install ansible -y**
 - finally, can varify to make sure ansible is properly installed on the master node.
 - **which ansible**
 - **ansible --version** 
 
  
  




- **1ST TASK:**
I LAUNCH 3 EC2 INSTANCES ON AWS. I CONFIGURED 1 EC2 INSTANCE AS AN ANSIBLE MASTER AND OTHER EC2 INSTANCES HAVE BEEN CONFIGURED AS A NODES.
NODE 1 IS RUNNING WITH UBUNTU O/S AND NODE 2 IS RUNNING ON REDHAT CENTOS O/S
ANSIBLE PLAYBOOK WILL CHECK THE O/S OF THE NODES IF ANY NODE IS RUNNING ON UBUNTU THEN SIMPLY INSTALL APACHE SERVER ON IT AND IF ANY SERVER
IS RUNNING ON CENTOS THEN INSTALL HTTPD SERVER ON THE NODE. FINALLY I DEFINE GATHER_FACTS == YES THAT WILL ALLOW TO REPLACE THE HOST PARAMETER 
BASE ON THE REQUIREMENTS.

- **2ND TASK:**
I CONGIFURE JAVA (JDK VERSION -8) ON BOTH OF THE NODES BY USING ANSIBLE MASTER THAT IS PREQUSITE TO INSTALL JENKINS ON THE NODES.

- **3RD TASK:**
AFTER SUCESSFULLY, INSTALLING JAVA ON THE NODES THEN I CONFIGURE/INSTALL JENKINS ON THE NODES AND FINALLY STARTING THE JENKINS SERVICE AND ENBALING IT TO START ON REBOOT.

- **EXCUTE PLAYBOOKS BY FOLLOWING COMMANDS:**
- ANSIBLE-PLAYBOOK OS-CHECKS.YML
- ANSIBLE-PLAYBOOK JAVA.YML
- ANSIBLE-PLAYBOOK JENKINS.YML
