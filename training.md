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

