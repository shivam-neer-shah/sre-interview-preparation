Question:

You are working at an organization that has just hired two new technicians. One of them will be the backup system administrator, while the other will need the ability to perform some tasks on the system with elevated privileges. You will create these two new accounts, and through the modification of the /etc/sudoers file and a separate sudoers file, these two new users will be able to invoke the sudo command.

Solutions:

Introduction
In this hands-on lab, we'll create new users that will be granted varying degrees of sudo access.

Solution
Open a terminal session, and log in via SSH using the credentials provided on the lab page.

Create Two New Users
Create a gfreeman user on the system:

sudo useradd -m gfreeman
Create an avance user, and assign it to the wheel supplemental group:

sudo useradd -G wheel -m avance
Set the password for both accounts to LASudo321:

sudo passwd gfreeman
sudo passwd avance
Verify the /etc/sudoers File and Test Access
Verify that the /etc/sudoers file will allow the wheel group access to run all commands with sudo:

sudo visudo
Note that there should not be a comment (#) on this line of the file:

%wheel  ALL=(ALL)       ALL
Switch to the avance account, and use the dash (-) to utilize a login shell:

sudo su - avance
Attempt to read the /etc/shadow file at the console:

cat /etc/shadow
Rerun the command with the sudo command:

sudo cat /etc/shadow
After you have verified avance can read the /etc/shadow file, log out of that account:

exit
Set Up the Web Administrator
Create a new sudoers file in the /etc/sudoers.d directory that will contain a standalone entry for webmasters:

sudo visudo -f /etc/sudoers.d/web_admin
Enter in the following at the top of the file:

Cmnd_Alias  WEB = /bin/systemctl restart httpd.service, /bin/systemctl reload httpd.service
Add another line in the file for gfreeman to be able to use the sudo command in conjunction with any commands listed in the WEB alias:

gfreeman ALL=WEB
Save and close the file with :wq!.

Next, log in to the gfreeman account:

sudo su - gfreeman
Attempt to restart the web service:

sudo systemctl restart httpd.service
Try to read the new web_admin sudoers file:

sudo cat /etc/sudoers.d/web_admin
Since the cat command is not listed in the command alias group for WEB, gfreeman cannot use sudo to read this file.

