Question:

You have been tasked with installing and testing software on 2 Linux servers.

You have been provided with a CentOS 7 Linux instance and an Ubuntu 20.04 Linux instance.

Your task is to install the software called ELinks on both of the servers that have been supplied.

You will need to create a text file with some sample text inside. Once this is complete, you will use the ELinks software to test that you can read the contents of the text file.

Once this has been completed, verify the software is installed and working.

Solution:

Overview of Linux: CentOS and Ubuntu
Introduction
In this hands-on lab, you will be provided with 2 Linux instances: 1 is CentOS, and 1 is Ubuntu. You will be installing software on both of these servers and will be testing if the software was properly installed and works.

Solution
Log In to the Provided Servers, and Ensure They Are Ready
Log in to the CentOS 7 server using the credentials provided:

ssh cloud_user@<PUBLIC_IP_ADDRESS_OF_CENTOS7>
Open a new terminal tab, and log in to the Ubuntu server using the credentials provided:

ssh cloud_user@<PUBLIC_IP_ADDRESS_OF_UBUNTU>
On Both Linux Servers, Install the Program Called ELinks
Go back to the CentOS terminal tab, and install ELinks. Enter your CentOS instance password, if requested:

sudo yum install elinks
When prompted, press y, and then ENTER.

Go to the Ubuntu terminal tab, and first update the database. Enter your Ubuntu instance password, if requested:

sudo apt update
Install ELinks on the Ubuntu instance:

sudo apt install elinks
When prompted, press y, and then ENTER.

Create a Text File with Sample Data Inside, and Use the ELinks Program to Display It on Each of the Linux Servers
On both the CentOS and Ubuntu terminal instances, create a file named test.txt with some sample text:

echo "Just text in this file" >> test.txt
Verify the file exists on both instances:

cat test.txt
You should see the file contents in the output.

Use the below command to open ELinks and view the files:

elinks test.txt
Run this on both the Ubuntu and CentOS terminal instances. You should see the file contents displayed in the output.

Press CTRL + C to exit ELinks.