## ansible-x 294 E-Practice 

### Configure Ansible Env

Install/Configure Ansible on Workstation machine with following settings:
+ On workstation machine create a user called neo with password ABcd!@# and su to user neo 

+ install ansible 2.9 on workstation 

+ create a static inventory file called /home/neo/ansible/inventory so that:
   1. servera is a member of dev host group
   2. serverb is a member of test host group 
   3. serverc and serverd is a member of prod group
   4. the prod group is a member of webservers host group 
   5. the dev and test group is member of the devtest group 
   6. servera and serverb is a member of blackmesa-east group 
   7. serverc and serverd is a member of blackmesa-north group 
   8. both blackmesa-east and blackmesa-north belongs to blackmesa group 

+ Create a configuration file called /home/neo/ansible/ansible.cfg which meets the following settings
   1. the host inventory points to /home/neo/ansible/inventory 
   2. the location of roles used in playbooks by user neo includes /home/neo/ansible/roles and /usr/share/ansible/roles 
   3. Maximum amount of fork is set to 2 for all playbook execution 
   4. default remote user is neo 
   5. remote user neo should able to perform privilege escalation on remote systems 

> Note: beyond this, your workstation machine will act as the ansible control node

### Configure remote systems 
Configure the managed host with the proper configuration: 
+ Create a playbook called init_user.yml that will create user neo with password ABcd!@# on all managed hosts 

+ Create a playbook called init_ssh.yml that will copy user neo ssh public key to all managed host so that user neo can perform passwordless ssh login to all managed host

+ Create a playbook called init_sudo.yml that will configure user neo to escalate to user root with no password prompt on all managed hosts

+ write ansible ad-hoc command that will setup yum repo to point to workstation and save it in a shell script called yum_repo_setup.sh , upon execution of the shell script, it should go to all managed host and create 2 yum repo file that points to workstation machine's RHEL Yum repo and Ansible Yum repo

### Install Packages
+ Create a playbook called init_pack.yml that will:
   1. installs php , vsftpd and  mariadb packages on host test and prod group
   2. installs Development Tools package group on host in the dev group
   3. update all packages to latest version on hosts in the dev group 

### Use System Role 
+ Install the RHEL system roles package and create a playbook called time.yml that will perform the following:
   1. Runs on all Managed hosts
   2. Uses the timesync role 
   3. Configure the role to use chrony as the default NTP provider 
   4. Configure the role to use the time server 2.asia.pool.ntp.org

### Configure Disk Automation 
+ Create a playbook called init_disk.yml that runs on all managed host and performs the following: 
   1. Creates a Volume group named bigdata that uses the additional disk with 5GB size ( DO NOT use O.S disk )
   2. Creates 2 logical volume called dbdata and webdata with 1GB size each 
   3. Formats the logical volume dbdata with ext4 filesystem and webdata with xfs filesystem 
   4. Persistently mounts webdata to /web 
   5. DO NOT mount logical volume dbdata in any way 

### Create Role
+ create a role called webstar in user neo default roles directory with the following requirement:
   1. httpd package is installed , enabled on boot and started
   2. The firewall is enabled and running with a rule that allow access to http port 
   3. A template file called index.html.j2 exist and is used to create default index.html page with following output 

   ```sh
   Welcome to BlackMesa-East
   You are serving machine: HOSTNAME on IPADDRESS
   SYSTEM memory is MEMORY
   ```

   >note: the HOSTNAME is the FQDN of the managed host and IPADDRESS is IP address of the managed host and MEMORY is the memory amount in Mb of the managed host
   4. Create a playbook called init_webstar.yml that uses role webstar that runs on all blackmesa-east managed system 

### Create Web Directory 
+ Create a playbook called init_webdata.yml with the following requirement
   1. The playbook runs on all blackmesa-east managed host
   2. Creates a directory called /webdev 
   3. The /webdev directory have owner permission rwx , group permission rwx and other with read and execute 
   4. Create a symbolic link /var/www/html/webdev to /webdev 
   5. Create a index.html inside /webdev that has single line: BLACKMESA-EAST
   6. Browsing this directory on blackmesa-east managed host should produce : 

   ```sh 
   BLACKMESA-EAST
   ```

### Create/Modify File content
+ Create a Playbook called init_file.yml that runs on all managed host, with the following requirement 
   1. Create a file called  /web/initfile.txt 
   2. The file content must have the ansible group name the managed host belongs to.
   >Example: in the managed host servera.lab.example.com, the content should read as:
   ```sh 
   dev
   devtest
   blackmesa-east
   blackmesa
   ```
   >Note: The above list is auto generated based on managed host membership, in the case of servera, based on ansible inventory, it belongs to dev, devtest , blackmesa-east and blackmesa host group ( refer Configure Ansible Env section )

### Ansible-vault 
+ Create a ansible vault to store user name and password as follows:
   1. the name of the ansible vault is locked.yml 
   2. the vault have 3 variables with names:
   ```sh 
      pass_dev with value NotailAna
      pass_blackmesa with value GordonFreeman
      pass_test with value SpecCarry
   ```
   3. The password to encrypt and decrypt the ansible vault locked.yml is DotaDataDita37
   4. The password should be stored in the file secdef.txt 


### User Management 
+ Create a playbook called ug.yml that will call the ansible vault file locked.yml (created on previous task) that will do the following:
   1. creates user called gordon and alex on all managed host using password value from variable pass_blackmesa 
   2. create user called tom and bob on all managed host using password value from variable pass_dev 
   3. user gordon and alex must belong to a group called blackmesa-g
   4. user tom and bob belongs to a group called dev-g 


### END

# More resources for exam prep: 

https://www.lisenet.com/2019/ansible-sample-exam-for-ex294/

https://networknuts.net/sample-exam-ex294/

