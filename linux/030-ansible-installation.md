Question:

Your CIO has greenlit a proof of concept for Ansible in your environment. You are to set up an Ansible control node in a test environment and verify basic functionality. You have three demo hosts, one to be the control node (control1), and two to serve as managed nodes (node1 and node2). You must complete the following steps:

1. Install Ansible on the control node.
2. Configure the ansible user on the control node for ssh shared key access to managed nodes.
Note: do not use a passphrase for the key pair.
3. Create a simple Ansible inventory on the control node in /home/ansible/inventory containing node1 and node2.
4. Configure sudo access for Ansible on node1 and node2 so that Ansible may use sudo for any command with no password prompt.
5. Verify each managed node can be accessed by Ansible from the control node using the ping module. Redirect the output of a successful command to /home/ansible/output.

Important Notes:

The user ansible is already present on all servers for your convenience.
The ansible user has the same password as the cloud_user.
/etc/hosts entries are present on control1 for the managed nodes.

Solution:

In this hands-on lab, we'll install Ansible on a control node and configure two managed servers for use with Ansible. We will also create a simple inventory and run an Ansible command to verify our configuration is correct.

This course is not approved or sponsored by Red Hat.

Install Ansible on the Control Node
Log in to the control node using ssh, cloud_user, and the provided public IP address and password:

ssh cloud_user@<PUBLIC IP>
To install Ansible on the control node:

sudo yum install ansible
Configure the ansible User on the Control Node
Next, we'll configure the ansible user on the control node for ssh shared key access to managed nodes.

Note: Do not use a passphrase for the key pair.

Create a key pair for the ansible user on the control host, accepting the defaults when prompted:

su - ansible
ssh-keygen
Copy the public key to both node1 and node2:

ssh-copy-id node1
ssh-copy-id node2
Create a Simple Ansible Inventory
Next, we'll create a simple Ansible inventory on the control node in /home/ansible/inventory containing node1 and node2.

On the control host:

su - ansible
touch /home/ansible/inventory
echo "node1" >> /home/ansible/inventory
echo "node2" >> /home/ansible/inventory
Configure sudo Access for Ansible
Now, we'll configure sudo access for Ansible on node1 and node2 such that Ansible may use sudo for any command with no password prompt.

Log in to node1 as cloud_user and edit the sudoers file to contain appropriate access for the ansible user:

ssh cloud_user@node1
sudo visudo
Add the following line to the file and save:

ansible    ALL=(ALL)       NOPASSWD: ALL
Enter:

logout
Repeat these steps for node2, and then back out to the control node.

Verify Each Managed Node Is Accessible
Finally, we'll verify each managed node is able to be accessed by Ansible from the control node using the ping module.

Redirect the output of a successful command to /home/ansible/output.

To verify each node, run the following as the ansible user from the control host:

ansible -i /home/ansible/inventory node1 -m ping
ansible -i /home/ansible/inventory node2 -m ping
To redirect output of a successful command to /home/ansible/output:

ansible -i /home/ansible/inventory node1 -m ping > /home/ansible/output
