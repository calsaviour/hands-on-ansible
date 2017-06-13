## Training/Hands on to create ansible

0. vagrant ssh acs
1.mkdir exercise1; cd exercise1

## Create Inventory Compoenent
2. vi inventory

## Basic Ansible Command ##
ansible <system> 
-i <inventoryFile>
-m <module>
-u <username>
-k <password prompt>
-v (-vv debug level2/ -vvv debug level3)

## Add the ip of the web server and db server
3. 192.168.33.20 , 192.168.33.33
4. ping the ip. 
eg ansible 192.168.33.20 -i inventory -u vagrant -m ping -k
   ansible all -i inventory -u vagrant -m ping -k
   ansible all -i inventory -u vagrant -m ping -k -vvv
   
   ansible all -i inventory -u vagrant -m command -a "/sbin/reboot"
   ansible all -i inventory -u vagrant -m command -a "/usr/sbin/yum update -y"
   or
   ansible all -i inventory -u vagrant -m -a "/usr/sbin/yum update -y"


5. Module has 2 types : command(run a python executable) and shell (can use shell variable)


## Inventory Fundamentals
1. Inventory Features - Behavioral parameters, Groups, Groups of Groups, Assign Variables, Scaling out using multiple files, Static/Dynamic Inventory File

## Example of Inventory File
web1 ansible_ssh_host=192.168.33.20 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant

run ansible web1 -i inventory -m ping


## Setting Group
web1 ansible_ssh_host=192.168.33.20 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
db1  ansible_ssh_host=192.168.33.30 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant

[webservers]
web1
db1


## Setting with Group of Groups

web1 ansible_ssh_host=192.168.33.20 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant
db1  ansible_ssh_host=192.168.33.30 ansible_ssh_user=vagrant ansible_ssh_pass=vagrant

[webservers]
web1

[dbservers]
db1


[datacenter:children]
webservers
dbservers

run command  ansible datacenter -i inventory -m ping


## Sharing variables
web1 ansible_ssh_host=192.168.33.20 
db1  ansible_ssh_host=192.168.33.30

[webservers]
web1

[dbservers]
db1


[datacenter:children]
webservers
dbservers

[datacenter:vars]
ansible_ssh_user=vagrant
ansible_ssh_pass=vagrant


run command ansible datacenter -i inventory -m ping


## Scaling out with Multiple Files
Using directories to manage

|__production
|	|
|	|--------group_vars
|	|	\all
|	|	\db	
|	|--------host_vars
|	|   \web1
|   |--------inventory_prod
|__test
|	|
|	|--------group_vars
|	|	\all
|	|	\db	
|	|--------host_vars
|	|   \web1
|   |--------inventory+test


Order of Operations (Precedence)
1. Group_Vars - All
2. Group_Vars - GroupName
3. Host_Vars Hostname (The highest precedence)

 --The most specific version of that variable is actually going to take the highest level of precedence.

-- variable files are written in YAML file

Eg of variable file

---
# file : group_vars/dc1-west
ntp: ntp-west.company.com
syslog: logger-west-company.com


## Demo : Scaling out with Multiple Files
Create the group variable username in the group_vars file

run command ansible webservers -i inventory_prod -m user -a "name={{username}} password=12345" --sudo


## Ansible Configuration
## To configure fingerprint for ssh
1. touch ansible.cfg
2. [defaults] host_key_checking=False
3. ansible web1 -i inventory_prod -m ping

## Ansible Environment Variable Takes Precedence over Ansible Configuration

1. export ANSIBLE_HOST_KEY_CHECKING=True
2. ansible web1 -i inventory_prod -m ping


## Ansible Modules.
There are 3 kinds of modules : Core, Extras and Deprecated

1. To list modile run ansible-doc -l
1. To list modile run ansible-doc -s <name>
1. To list modile run ansible-doc <name>

### Copy Module
Copies a file from local box to remote system
Has "backup" capability
Can do validation remotely

### Fetch Module
Pulls a file from remote host to local system
Can use md5 checksums to validate

### Apt Module
Manages installed applications on Debian-based systems
Can install, update or delete packages
Can update entire system

### Yum Module
Manages installed applications on Redhat-based systems
Can install, update or delete packages
Can update entire system

### Service Module
Can stop, start and restart services
Can enable services to start on boot


