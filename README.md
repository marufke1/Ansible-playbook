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
 - after that we can install ansible on the master server by following this command
 - **yum install ansible -y**
 - finally, can varify to make sure ansible is properly installed on the master node.
 - **which ansible**
 - **ansible --version**
   

 - **STEP 2:** After installing ansible on the master server that will create Ansible INVENTORY and CONFIGURATION file by default.
 
 - **Inventory file:** Inventory file is the default file for ansible. It is also called hosts file as well. Inventory file is going to allow to store
   private ip address of the nodes so that ansible master exactly knows whatever nodes need to be configured. Asible creates an inventory file by using
   default location which is **/etc/ansible/hosts**. we can store the private ip address of the nodes inside of the default inventory file.
 - We also can create an inventiory file then store the nodes private ip inside of it by following commands.
 - **vi Inventroy**
 - we can define the group name for the nodes which is
 - **[Devops]** ### this group has 2 nodes.
 - 172.31.8.50 ### node 1
 - 172.31.1.228 ### node 2
 - finally ansible master server has the information of the nodes need to be configured.


 - **STEP 3:** Updating the configuration file is an optional only require when we store the private ip of the nodes inside of the default inventory file.
 - default configuration file location is: **/etc/ansible/ansible.cfg**
 - we need to make some changes insode of this configuration file that allows to **uncomment the inventory file and sudo user.**
 - so that ansible master can successfully configure the nodes.


 - **STEP 4:** then we create a ansible user for the master and nodes that means can configure the nodes by using ansible user.
 - root user is the main user which has sudo privilege and only use for administrative purposes.
 - command for creating a new user:
 - **useradd ansible**
 - **passwd ansible**
 - **su - ansible ## allows to switch the user to ansible.**
 - must provide sudo previlege for ansibel user by following command
 - visudo #### need to make this changes by using root.
 - **ansible user ALL=(ALL) NOPASSWD: ALL**  #### allows to provide sudo permission to the anisble user for configuring the nodes.
 - varify to make sure ansible user has sudo permission
 - ** su - ansible**
 - **sudo yum install httpd -y**
    


 - **STEP 5:** Now, we can establish a SSH connection between master and nodes. We have to make some changes for the master and nodes server
   that allows to establish a successfully connection between of them.
 - **vi /etc/ssh/sshd_configure**  ### make some changes inside of this file
 - few things need to be commented and uncommented for further processing.
 - **permit root login**
 - **passwd auth = yes**
 - **##passwd auth = no**
 - switch the user by following command.
 - **su - ansible**
 - after that we need to restart the sshd configuration for all the nodes by following command
 - **service sshd restart**
 - finally we can establish a ssh connection between master and nodes by following commands.
 - **ssh 172.31.8.50 #### connects with node 1**
 - pass: XXX
 - **ssh 172.31.1.228 #### connects with node 2**
 - pass: XXX


 - **STEP 6:** Whenever we are going to establish a connection between master and modes it will ask for password authentication. Simply can solve this problem
   to generate a ssh key pair on the master node which has private and public key then need to copy the public key to the nodes that allow to acces from master to
   without having any password authentication.
 - **su - ansible**
 - **ssh-keygen #### will generate ssh key pair**
 - .ssh hidden file will generate
 - copy the public key from master to nodes  by following command
 - **ssh-copy-id ansible@172.31.8.50 #### node 1**
 - **ssh-copy-id ansible@172.31.1.228 #### node 2**
 -  Varify to connect between master and nodes
 - **ssh 172.31.8.50 ##### it will directly establish a connection without having any password authention**
   


 - **DIFFERENT TYPES OF COMMANDS:** There are 3 different types of command available for Ansible to configure the nodes.
   
  1. **Ansible ADHOC command** ADHOC command is a simple linux shell command. ADHOC command commonly use to perform single task on the nodes.
     Different types of configuration can be done by using ad hoc commands like-
     - creating a file on the nodes
     - creating a directory
     - removing file and directory
     - listing the files and dir
     - checking the memory and storage for the nodes 
     - installing packages on the nodes etc... **(normally don't use adhoc command to install any packages on the nodes.)**
     - **Ad hoc commands exaples:**
     - **ansible -i inventory all -a "touch file1"**
     - **ansible -i inventory all -a "ls"**
     - **ansible -i inventory all -ba "yum install httpd -y"**
     - **ansible -i inventory all -a "df -h"**
    

 2. **Ansible MODULE command** Module command use to perform single task on the nodes like ad hoc command but the main difference is to define the
    task for the nodes by using yml language. when we define multiple modules that becomes a playbook. Module can perform any types of configuration
    but mostly used for installing packages on the nodes.
    - creating a file on the nodes and other simple tasks as well
    - installing different packages on the nodes but only if we want to perform a single taks like install httpd on the nodes.
    - **Module commands examples:**
    - **ansible -i inventory -m "shell" -a "touch file1" ### m = is for module we define which is shell allows to use touch command to creat a file.**
    - **ansible -i inventroy -m "shell" -a "mkdir Devops"**
    - **ansible -i inventory -m "shell" -a "free -m"**
    - **ansible -i inventory -b -m "yum" -a pkg=httpd state=present**
   
      
3. **Ansible Playbook command:** Ansible playbook command use to perform multiple tasks on the nodes like installing httpd and then starting httpd on the
   server. ansible playbook is written by yml language which is very easy to understand and human redable. when we define multple module for the task then
   it becomes a playbook. Ansible playbook can perform different tasks on the nodes as well like- create a file and directory etc...
   - installing different packages on the nodes but only if we want to perform a multiple tasks like install and starting httpd server on the nodes.
     
   - **Ansible play-book example:**
   - **vi playbook.yml ### must use yml extension for the ansible file/playbook**
     
   - **--- ## always start with three dashes that indicates this is yml file**
   - **Target group info ## allows to provide the info about the play book like name, hosts, user, become, gather_facts etc..**
     
          - **name: install and starting nginx on the nodes**
        
            **hosts: Devops**
       
            **user: ansible**
       
            **become: yes ### provides root permission for the ansible user to perform the tasks on the nodes**
       
            **connection: ssh**
       
            **gather_facts: yes ### gather the private ip addresses of the nodes**

       **tasks: ### provides the tasks information need to be performed on the nodes.**
  
            **tasks:

                - name: install nginx on the ndoes
                  yum:
                    name: nginx
                    state: present

                - name: start nginx on the nodes
                  service:
                    name: nginx
                    state: started

       **Execution: finally execute the play book that allows to install and start the nginx on the nodes**
       **Execution command** **ansible-playbook -i inventory playbook.yml**


4. **Ansible Role:** Ansible role is going to allow to write the playbook in more efficient way for an example: we are writing a play book
   where defines so many tasks, variables, handlers, condition etc that creates a playbook very large even it will be very difficult to find
   out which task is being configured for what purposes. We easily can define roles for specific component and then just define the role name
   in the playbook that allows to configure the tasks on the nodes. 

   **Ansible roles example:
   - **first I am going to create a parent directory by using follwing command mkdir -p playbook/roles/webserver/tasks**
   - **change the directory cd playbook/**
   - **create a file which is main.yml inside of the tasks directory and defines the tasks need to be performed on the nodes**
   -  **vi playbook/roles/webserver/tasks/main.yml**
     
   -  **tasks: provides the tasks information**
     
           -  name: install nginx on the nodes
              yum:
                name: nginx
                state: present
      
           -  name: start nginx on the nodes
              service:
                name: nginx
                state: started

   - **create an another file which is master.yml that contains the information like- target group and role names**
     
   - **vi master.yml**
  
   - **---**
   -  **name: install and starting niginx on the nodes**
      **hosts: Devops**
      **user: ansible**
      **become: yes**
      **connection: ssh**
      **gather_facts: yes**
      **roles:**
            **- webserver**

   - **Execution: finally execute the master.yml playbook to perform apply the tasks on the nodes**
   - **Execution command: ansible-playbook -i inventory master.yml**

    
   - **PROJECTS:**


   - **1ST TASK:**
   I LAUNCH 3 EC2 INSTANCES ON AWS. I CONFIGURED 1 EC2 INSTANCE AS AN ANSIBLE MASTER AND OTHER EC2 INSTANCES HAVE BEEN CONFIGURED AS A NODES.
   NODE 1 IS RUNNING WITH UBUNTU O/S AND NODE 2 IS RUNNING ON REDHAT CENTOS O/S ANSIBLE PLAYBOOK WILL CHECK THE O/S OF THE NODES IF ANY NODE
   IS RUNNING ON UBUNTU THEN SIMPLY INSTALL APACHE SERVER ON IT AND IF ANY SERVER IS RUNNING ON CENTOS THEN INSTALL HTTPD SERVER ON THE NODE.
   FINALLY I DEFINE GATHER_FACTS == YES THAT WILL ALLOW TO REPLACE THE HOST PARAMETER BASE ON THE REQUIREMENTS.

   - **2ND TASK:**
   I CONGIFURE JAVA (JDK VERSION -8) ON BOTH OF THE NODES BY USING ANSIBLE MASTER THAT IS PREQUSITE TO INSTALL JENKINS ON THE NODES.

   - **3RD TASK:**
   AFTER SUCESSFULLY, INSTALLING JAVA ON THE NODES THEN I CONFIGURE/INSTALL JENKINS ON THE NODES AND FINALLY STARTING THE JENKINS SERVICE AND
   ENBALING IT TO START ON REBOOT.


  - **EXCUTE PLAYBOOKS BY FOLLOWING COMMANDS:**
  - **ANSIBLE-PLAYBOOK OS-CHECKS.YML**
  - **ANSIBLE-PLAYBOOK JAVA.YML**
  - **ANSIBLE-PLAYBOOK JENKINS.YML**
