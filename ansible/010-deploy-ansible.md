Question:

You have been tasked with putting together a presentation to demonstrate how Ansible may be used to install software on remote hosts automatically. Before the demo, you will need to configure your test systems. You have been provided two hosts called control and workstation. You will need to configure the ansible user on workstation to have sudo access without a password to the automated software installed via Ansible. You must also configure the control host as your Ansible control server by installing Ansible on it as well as configuring the Ansible user with a pre-shared key to login to the workstation host as the ansible user.

Once the basic configuration is complete, you will need to create a simple inventory in /home/ansible/inventory on the control server containing the workstation host. Afterward, you will write a simple playbook in /home/ansbile/git-setup.yml on the control host that installs git on the workstation host. You will need to make sure the playbook works by running it from the control server.

Summary tasks list:

Install Ansible on the control host.

Create an ansible user on both the control host and workstation host.

Configure a pre-shared key for Ansible that allows the user to log in from control to workstation without a password.

Configure the Ansible user on the workstation host so that Ansible may sudo without a password.

Create a simple inventory in /home/ansible/inventory consisting of only the workstation host.

Write and execute an Ansible playbook in /home/ansible/git-setup.yml on the control node that installs git on the workstation host.

=========
Solution:
=========

We have been tasked with putting together a presentation to demonstrate how Ansible may be used to install software on remote hosts automatically.

Install Ansible on the Control Host
Log in to the Control Host server using ssh, cloud_user, and the provided Public IP address and password:

ssh cloud_user@<PUBLIC IP>
Install epel-release and enter y when prompted:

sudo yum install epel-release
Install Ansible and enter y when prompted:

sudo yum install ansible
Create an ansible User
Now, we need to create the ansible user on both the control host and workstation host.

On the control node:

sudo useradd ansible
Connect to the workstation node using the provided password:

ssh workstation
Add the ansible user to the workstation and set a password for the ansible user. We need to make sure it is something we will remember:

sudo useradd ansible
sudo passwd ansible
logout
Configure a Pre-Shared Key for Ansible
With our user created, we need to create a pre-shared key that allows the user to log in from control to workstation without a password.

Change to the ansible user:

sudo su - ansible
Generate a new SSH key, accepting the default settings when prompted:

ssh-keygen
Copy the SSH key to workstation, providing the password we created earlier:

ssh-copy-id workstation
Test that we no longer need a password to log in to the workstation:

ssh workstation
Once we succeed at logging in, log out of workstation:

logout
Configure the Ansible User on the Workstation Host
Our next job is to configure the ansible user on the workstation host so that that they may sudo without a password.

Log in to the workstation host as cloud_user using the password provided by the lab:

ssh cloud_user@workstation
Edit the sudoers file:

sudo visudo
Add this line at the end of the file:

ansible       ALL=(ALL)       NOPASSWD: ALL
Save the file:

:wq
Log out of workstation:

logout
Create a Simple Inventory
Next, we need to create a simple inventory, /home/ansible/inventory, consisting of only the workstation host.

On the control host, as the ansible user, run the following commands:

vim /home/ansible/inventory
Add the text "workstation" to the file and save using :wq in vim.

Write an Ansible Playbook
We need to write an Ansible playbook into /home/ansible/git-setup.yml on the control node that installs git on workstation, then executes the playbook.

On the control host, as the ansible user, create an Ansible playbook:

vim /home/ansible/git-setup.yml
Add the following text to the file:

--- # install git on target host
- hosts: workstation
  become: yes
  tasks:
  - name: install git
    yum:
      name: git
      state: latest
Save and exit the file (:wq in vim).

Run the playbook:

ansible-playbook -i /home/ansible/inventory /home/ansible/git-setup.yml
Verify that the playbook ran successfully:

ssh workstation
which git
