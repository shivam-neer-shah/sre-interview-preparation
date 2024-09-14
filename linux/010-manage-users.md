Managing Users in Linux
This course is not approved or sponsored by Red Hat.

Introduction
In this lab we are going to manage users and groups to get some practice using these utilities. Knowing how to manage users and permissions means our servers will be more secure.

Solution
Log in to the server using the credentials provided:

ssh cloud_user@<PUBLIC_IP_ADDRESS>
Become root:

sudo -i
Enter the cloud_user password at the prompt.

Add the Users to the Server
Add our users:

useradd tstark
useradd cdanvers
useradd dprince
Create the superhero Group
Create the new group:

groupadd superhero
Set wheel Group as the the tstark Account's Primary Group
The usermod command will change which group a user is in. Change tstark:

usermod -g wheel tstark
Make sure it worked:

id tstark
The command's output should show his primary group is now wheel.

Add superhero as a Supplementary Group on All Three Users
Run the usermod command for each user:

usermod -aG superhero tstark
usermod -aG superhero dprince
usermod -aG superhero cdanvers
Check with any of the users to make sure it worked:

id <USERNAME>
We should see they're now in superhero, as well as their own groups.

Lock the dprince Account
Run the following:

usermod -L dprince