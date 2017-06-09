** Example for Creating Ansible Environment **
** Source : Hands on Ansible by Aaron Paxson


1. vagrant init
2. vagrant up
3. vagrant ssh acs

Step 4. Install in Ubuntu
sudo apt-get install ansible -y ##install ansible

Step 4. Install in Centos
vagrant ssh web
sudo yum install epel-release ## install enterprise linux packages
sudo yum install ansible


Step 4. Install in Centos DB server. Compiling the code
** Check GCC or GNU Compiler is installed **
sudo yum install gcc -y

** Install python setup tools **
sudo yum install python-setuptools

** Install repository to get python packages **
sudo easy_install pip

** Install python source code headers **
sudo yum install python-devel

** Get ansible code and compile it **
sudo pip install ansible

** Encounter error when running above command. Try updating python
https://tecadmin.net/install-python-2-7-on-centos-rhel/#

**Check running vm"**
1. vboxmanage list runningvms