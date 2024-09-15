Question:

Before we can work with a system, we need to access it first. A Linux system administrator works with a number of different user accounts in the course of their regular day. Understanding how to leverage different user accounts and their associated environments is a key skill.

Since most system logins won't be at the console, the SSH utility is a key tool in any Linux system administrator's toolbox. It allows users to not only make secure connections to other hosts but also securely execute commands on remote systems, securely tunnel services and X11 sessions, transfer files, and more.

Using passwords for user authentication has been the standard for a long time. But what if you need to make connections or execute remote commands without interaction? SSH provides a mechanism for this via key-based authentication.

Red Hat Exam Requirements Covered:

Log in and switch users in multi-user targets
Access remote systems using SSH
Configure key-based authentication for SSH
Securely transfer files between systems


Solution

Introduction
In this hands-on lab, we will take a look at the su command, explore the power of the ssh command, configure key-based authentication for SSH, and take a look at the SSH utilities for secure file transfer.

Solution
Log in and Switch Users
Log in to the server using the credentials provided:

ssh cloud_user@<PUBLIC_IP_ADDRESS>
To get information on who we are, use:

whoami ; groups
For more information on our user ID, primary group ID, and groups that we're part of, run this command:

id
To determine what files get run when we use different commands to elevate our privileges, become the root user:

sudo -i
Enter the following command to export and echo SOURCED1 into the root user's bash_profile:

echo export SOURCED1=.bash_profile >> ~/.bash_profile ; echo 'echo $SOURCED1' >> ~/.bash_profile
Make sure both lines are there:

grep SOURCED .bash_profile
Enter the following command to export and echo SOURCED2 into the root user's .bashrc:

echo export SOURCED2=.bashrc >> ~/.bashrc ; echo 'echo $SOURCED2' >> ~/.bashrc
Make sure both SOURCED lines are there:

grep SOURCED .bashrc
Exit by simply entering:

exit
Kill the timeout:

sudo -k
To see the echoes, enter:

sudo -i echo
Type in the cloud_user's password to see the echoes.

Set the root user's password:

sudo -i passwd root
Enter a new password for the root user.

Run the command to see the cloud_user's path:

su -c 'echo $PATH'
Enter the root user's password. This should show the cloud_user's path.

Run the command to sign in as the root user and show the cloud_user's path:

su - -c 'echo $PATH'
Enter the root user's password. This should show the root user's profile, bashrc, and path.

Understand that sudo equals the cloud_user, but sudo -i equals the root user. Su also equals the cloud_user, while su - equals the root user.

Access Remote Systems Using SSH
Connect to a remote system on the second cloud server:

ssh cloud_user@<SECOND_PUBLIC_IP_ADDRESS>
When asked if you want to continue connecting, type "yes".

Enter the password for the remote system on the second cloud server.

Change the password:

sudo -i passwd cloud_user
Enter first the old password and then a new password that you'd prefer to use.

To get some information on the remote system, query the remote system by running the following commands:

top
hostname
df -hT
Exit out of the server:

exit
Return the remote system's output to the initial system:

ssh -t cloud_user@<SECOND_PUBLIC_IP_ADDRESS> df -hT >> server_health.txt
Enter the password for the remote system on the second cloud server.

If you see an error message, pull up the server health file:

cat server_health.txt
See the free memory on the server with the free command:

ssh -t cloud_user@<SECOND_PUBLIC_IP_ADDRESS> free >> server_health.txt
Enter the password for the remote system on the second cloud server.

Get the information and see the text file about the server health file:

cat server_health.txt
Configure Key-Based Authentication for SSH
Generate a public/private key pair using the defaults on the first cloud server:

ssh-keygen
Hit Enter and type in a passphrase, ideally something that's easy to remember. This should give you your randomart image and ID.

Copy that ID to the second cloud server, also known as the remote server:

ssh-copy-id <SECOND_PUBLIC_IP_ADDRESS>
Type in the password for the second cloud server.

Connect to the remote server:

ssh cloud_user@<SECOND_PUBLIC_IP_ADDRESS>
Type in the passphrase that you created a few steps ago to test if the key is working.

Exit out of the remote server:

exit
Add the cloud_user identity to the agent and to reload the agent:

eval $(ssh-agent -s)
Add your cloud_user identity to the agent, which can now act on your behalf:

ssh-add
Type in your passphrase.

Connect to the remote server:

ssh cloud_user@<SECOND_PUBLIC_IP_ADDRESS>
Securely Transfer Files between Systems
Execute a backup command on a remote system:

ssh cloud_user@<SECOND_PUBLIC_IP_ADDRESS> tar -czvf wget-server2.tar.gz wget-1*.rpm
Hit the Up arrow and perform an scp:

scp cloud_user@<SECOND_PUBLIC_IP_ADDRESS>:~/wget-server2*.* .
Check the home directory to make sure all the files have been transferred:

ls -l