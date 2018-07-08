# Comcast (Setup and Install SSH Key Auth. using Ansible on cluster of servers


Task is to create an Ansible PlayBook that will add users to cluster of servers and add their public SSH key allowing them to login securely.

**Prerequisites**

Cluster of Ubuntu Servers

10.0.15.10      control-machine

10.0.15.21      ansi01

10.0.15.22      ansi02

Root privileges

**Steps executed to achieve this tasks**

Setup Ansible Control Machine

Define User and SSH Key

Create Inventory File

Create Ansible Playbook

Deploy Server Using Playbook


**Configure and setup Ansible Control Machine**

For this task using,Ubuntu servers as Ansible 'Control machine' and Ansible hosts. 

Set up the 'control machine',
After the installation is complete, we will add a new system user.

We will add a new user named 'provision' in order to perform server provisioning using Ansible.

Add new user 'provision' and give the user a password.

`useradd -m -s /bin/bash provision
passwd provision`

Now add the 'provision' user for sudo without the password by creating new configuration file under the '/etc/sudoers.d/' using the command below.

`echo  -e 'provision\tALL=(ALL)\tNOPASSWD:\tALL' > /etc/sudoers.d/provision`

A new user has been created, and now it can use sudo without a password.

**Now Define User and SSH Key**

In this step, we will define the user for ansible hosts. This user will be automatically created by ansible, so we just need to define the username, password, and the ssh public key.

For each server ('ansi01' and 'ansi02'), we will create a new user named 'provision' with password 'secret01'. And we need to encrypt the 'secret01' password using the mkpasswd command.

Encrypt the 'secret01' password using the command below.

`mkpasswd --method=SHA-512
TYPE THE PASSWORD 'secret01'`

Note:

Make sure the 'whois' package is installed on the system, or you can install using the following command.

`sudo apt install whois -y
`
And you will get the SHA-512 encrypted password.

Define User and SSH Key

Next, we will generate a new ssh-key.

Login to the 'provision' user and generate the ssh key using the ssh-keygen command.

`su - provision
ssh-keygen -t rsa`

Now the user and password have been defined, and the ssh key has been created located at the '.ssh' directory).

**Create New Inventory**

In this step, we will define the inventory files for all server hosts.

Login as the 'provision' user and create a new directory for the project.

`su - provision
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
`inventory = /home/provision/ansible01/inventory.ini
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

On the playbook script:

we create the 'deploy-ssh.yml' playbook script to be applied on all servers defined in the 'inventory.ini' file.
we create the ansible variable 'provision_password', containing the encrypted password for the new user.
Set the Ansible facts to 'no'.
Define the 'root' user as a remote user to perform tasks automation.
We create new tasks for adding a new user, add the user to the sudoers, and upload the ssh key.
We create new tasks for configuring the ssh services, disabling the root login, and disable password authentication. Tasks for configuring the ssh will trigger the 'restart ssh' handlers.
We create a handler to restart the ssh service.

**Run the Playbook**

Login to the 'provision' user and go to the 'ansible01' directory.

`su - provision`

`cd ansible01/`

Now run the the 'deploy-ssh.yml' playbook using the command as shown below.

`ansible-playbook deploy-ssh.yml --ask-pass`

Tasks for deploying a new user and ssh key have been completed successfully.