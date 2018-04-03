# FSND_Linux_Server
IP Address: 35.170.61.252

SSH Port: 2200

URL: 

# Course Instructions:
1. Start a new Ubuntu Linux server instance on Amazon Lightsail.
2. Follow the instructions provided to SSH into your server.
3. Update all currently installed packages.
4. Change the SSH port from 22 to 2200. Make sure to configure the Lightsail firewall to allow it.
5. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port    123). (Warning: When changing the SSH port, make sure that the firewall is open for port 2200 first, so that you don't lock yourself out of the server.)
6. Create a new user account named grader.
7. Give grader the permission to sudo.
8. Create an SSH key pair for grader using the ssh-keygen tool.
9. Configure the local timezone to UTC.
10. Install and configure Apache to serve a Python mod_wsgi application. (If you built your project with Python 3, you will need to install the Python 3 mod_wsgi package on your server: sudo apt-get install libapache2-mod-wsgi-py3.)
11. Install and configure PostgreSQL: 
    Do not allow remote connections
    Create a new database user named catalog that has limited permissions to your catalog application database.
12. Install git.
13. Clone and setup your Item Catalog project from the Github repository you created earlier in this Nanodegree program.
14. Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser. Make sure that your .git directory is not publicly accessible via a browser!

# Steps done to complete project:
	• login to the lightsail instance using the SSH method from your browser

# Update All currently installed packages:
	• sudo apt-get update
	• sudo apt-get upgrade 
		Do you want to continue? Y
		When prompted choose "install the package maintainer's version"
	• sudo apt-get autoremove 
		Do you want to continue? Y

# Create a new user account named grader:
	• sudo su
		Now you are accessing the VM as the root user
	• passwd ubuntu 
		Set the password for user ubuntu
	• exit
		Now you are accessing the VM as ubuntu again
	• sudo adduser grader
		Enter new UNIX password for user grader
		Enter Full Name (I chose "Udacity Linux Grader")
		You can leave all other fields blank
		Is the information correct? Y

# Give grader the permission to sudo:
	• sudo usermod -aG sudo grader 
		This makes grader a super user
	• sudo su - grader
		To access VM as grader
	• sudo adduser menu
		Enter password for grader
		Create password for Menu
		Enter Full Name (I chose "Udacity Menu Project")
		You can leave all other fields blank
		Is the information correct? Y
	
# Change the SSH port from 22 to 2200:
   	while still logged in as grader (or run sudo su - grader):
	• sudo nano /etc/hosts 
		Place your "#STATIC IP# ubuntu" under "127.0.0.1 localhost"
		CTRL X
		Y
		ENTER
	• sudo nano /etc/ssh/sshd_config 
		Change port 22 to 2200
		Change PermitRootLogin to no
		Change PasswordAuthentication to yes
		CTRL X
		Y
		ENTER

# Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123):

	• sudo ufw status
	• sudo ufw default deny incoming
	• sudo ufw default allow outgoing
	• sudo ufw allow 2200/tcp
	• sudo ufw allow 80/tcp
	• sudo ufw allow 123/udp
	• sudo ufw allow www
	• sudo ufw allow ssh
	• sudo ufw enable
	• sudo ufw status
	• sudo service ssh restart

# Create an SSH key pair for grader using the ssh-keygen tool:
	• sudo su - grader
	• mkdir .ssh
	• touch .ssh/authorized_keys
	• nano .ssh/authorized_keys
		Copy the public key generated into the lightsail notepad
		Then CTRL+SHIFT+V to paste into the terminal
		on your local machine to this file and save
		CTRL X
		Y
		ENTER
	• chmod 700 .ssh
	• chmod 644 .ssh/authorized_keys
	• sudo service ssh restart
		Now you can log in via ssh for your terminal (ssh -i /.ssh/id_rsa grader@YOUR_LIGHTSAIL_IP_HERE -p 2200)	

# Configure the local timezone to UTC.

	• sudo dpkg-reconfigure tzdata

# Install and configure Apache to serve a Python mod_wsgi application.

# Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser.
