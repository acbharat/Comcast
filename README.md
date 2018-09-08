# MockPrj (Setup and Install SSH Key Auth. using Ansible on cluster of servers


Task is to create an Ansible PlayBook that will add users to cluster of servers and add their public SSH key allowing them to login securely.

**Prerequisites**

Cluster of Ubuntu Servers

10.0.15.10      control-machine

10.0.15.21      ansi01

10.0.15.22      ansi02

Root privileges

**Steps executed to achieve this task**

Setup Ansible Control Machine

Define User and SSH Key

Create Inventory File

Create Ansible Playbook

Deploy Server Using Playbook


**Configure and setup Ansible Control Machine**

Configuring Ubuntu servers as Ansible 'Control machine' and Ansible hosts. 

Set up the 'control machine'

After the installation is complete, we will add a new system user.

We will add a new user named 'ec2-user' in order to perform server provisioning using Ansible.

Define User and SSH Key

Next, we will generate a new ssh-key.

Login to the 'ec2-user' user and generate the ssh key using the ssh-keygen command.

`su - ec2-user
ssh-keygen -t rsa`

Now the user and password have been defined, and the ssh key has been created located at the '.ssh' directory.

**Create New Inventory**

In this step, we will define the inventory files for all server hosts.

Login as the 'ec2-user' user and create a new directory for the project.

`su - ec2-user
mkdir -p ansible01/`

Go to the 'ansible01' directory and create a new inventory file 'inventory.ini' using vim.

`cd ansible01/
vim inventory.ini`

Paste the following configuration there.

[webserver]

ansi01 ansible_host=10.0.15.21
 
ansi02 ansible_host=10.0.15.22
 
Save and exit.

Now create a new ansible configuration file 'ansible.cfg'.

`vim ansible.cfg`

Paste the following configuration there.

`[defaults]
`
`inventory = /home/ec2-user/ansible01/inventory.ini
`
Save and exit.

The ansible inventory file has been created, and our ansible scripts will be located under the 'provision' user, inside the 'ansible01' directory.

**Create Ansible Playbook**

In this step, we will create a new ansible playbook to deploy host list and then manually scan the ssh key fingerprint using bash script as shown below.

`for i in $(cat list-hosts.txt)
do
ssh-keyscan $i >> ~/.ssh/known_hosts
done`

Those servers fingerprint will be stored at the '.ssh/known_hosts' file.

Next, create the ansible playbook named 'deploy-ssh.yml' using vim.

`vim deploy-ssh.yml
`
Save and exit.

**Run the Playbook**

Login to the 'ec2-user' user and go to the 'ansible01' directory.

`su - ec2-uesr`

`cd ansible01/`

Now run the the 'deploy-ssh.yml' playbook using the command as shown below.

`ansible-playbook deploy-ssh.yml --ask-pass`

Tasks for deploying a new user and ssh key have been completed successfully.