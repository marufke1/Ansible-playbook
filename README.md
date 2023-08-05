**ANSIBLE PLAYBOOK PROJECT 1 DESCRIPTION:****

**CONFIGURATION ON THE EC2 INSTANCES:**
- FIRST INSTALL ANSIBLE ON THE MASTER NODE.
- AFTER SUCCESSFULLY INSTALLATION ANSIBLE ON THE MASTER WILL CREATE A INVENTORY FILE IN THE LOCAL REPO THAT USUALLY CONTAINS THE NODES GROUP NAME AND IP ADDRESS
- VI/ETC/ANSIBLE/HOSTS (TO CREATE THE GROUP NAME AND NODES IP ADDRESS)
- THEN I EDIT THE CONFIGURATION FILE THAT WILL ALLOW TO UNCOMMENTED THE INVENTORY FILE AND SUDO-USER =ROOT
- CREATE A USER ID AND PASSWORD FOR ALL THE NODES AND MASTER TOO.
- I GIVE SUDO PERMISSION TO THE USER SO THAT ANSIBLE USER CAN BE ABLE TO CONFIGURE THE NODES WITH OUT HAVING ROOT. (ANSIBLE ALL=(ALL) NOPASSWD:ALL
- THEN I ESTABLISH A SSH CONNECTION BETWEEN ANSIBLE MASTER AND NODES SO THAT WE CAN EASILY CONFIGURE ALL THE NODES BY USING ANSIBLE MASTER
- EDIT THE FILE VI/ETC/SSH/SSHD_CONFIG (UNCOMMENTED PERMIT ROOT LOGIN AND PASSWD AUTHETICATION =YES)
- FINALLY GENERATE  SSH KEY SO THAT USER DOES NOT NEED TO ACCESS THE ANSIBLE SERVER EVERYTIME WITH THE PASSWORD. 

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
