Question:

You have been asked by management to learn more about Docker. They heard that using Docker is easy to get a Linux instance up and running to use.

To test this, you have been asked to start up an Ubuntu Linux container and then log in to it. This is to ensure the container is working and is an Ubuntu Linux instance.

You have been provided with a CentOS 7 Linux instance that has Docker preinstalled.

Solution:

Introduction
In this hands-on lab, you will be provided a single CentOS Linux instance. Docker is preinstalled on this instance for your use. You have been asked by management to learn more about Docker. They heard that using Docker is easy to get a Linux instance up and running to use. To test this, you have been asked to start up an Ubuntu Linux container and then log in to it. This is to ensure the container is working and is an Ubuntu Linux instance.

Solution
Log In to the Provided Server, and Ensure It Is Ready
Log in to the server using the credentials provided:

ssh cloud_user@<PUBLIC_IP_ADDRESS>
It may take a few minutes after the lab starts before you can use Docker.

List the files in the home folder:

ls
If you see a file called ready.txt, then the lab is ready. If you don't see this file, then log out, wait 2 minutes, and log back in.

Using Docker, Run an Ubuntu Container
Check you can see Docker:

docker ps
You should see the Docker processes (CONTAINER ID, IMAGE, etc.) listed in the output, which means Docker is ready.

Start up the Ubuntu container in Docker:

docker run -it ubuntu bash
You should see Docker downloaded from the repository.

Connect to the Ubuntu Container and Test It Is Working
Test to see the container is running Ubuntu:

cat /etc/issue
You should see the Ubuntu version listed in the output.

Type exit to return to the CentOS instance.