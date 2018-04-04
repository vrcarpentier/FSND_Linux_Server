# FSND_Linux_Server
This README is for my Udacity Full Stack Nanodegree Linux Server Configuration Project. 

Public IP: 35.171.169.185

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
	• download SSH key pair from lightsail insatance account page
	• ssh -i ~/.ssh/udacity_key.pem ubuntu@public_ip_address

# Update All currently installed packages:
	• sudo apt-get update
	• sudo apt-get upgrade 
		Do you want to continue? Y
		When prompted choose "install the package maintainer's version"
	• sudo apt-get autoremove 
		Do you want to continue? Y

# Create a new user account named grader:
	• sudo adduser grader 
		Create password for grader
		Enter Full Name (I chose "Udacity Linux Grader")
		You can leave all other fields blank
		Is the information correct? Y
		
# Give grader the permission to sudo:
	• sudo vim /etc/sudoers
		i to get into insert mode
		add grader ALL=(ALL) ALL
		esc to exit insert mode
		:wq!
		
# Change the SSH port from 22 to 2200:
   	• sudo su - grader
	• sudo nano /etc/hosts 
		Place your "127.0.1.1 ip-10-20-52-12" under "127.0.0.1 localhost"
		CTRL X
		Y
		ENTER
	• sudo nano /etc/ssh/sshd_config 
		Change port 22 to 2200
		CTRL X
		Y
		ENTER
	• disable ssh login for root user
		sudo nano /etc/ssh/sshd_config
		change PermitRootLogin to no
		sudo service ssh restart

# Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123):

	• sudo ufw status
	• sudo ufw default deny incoming
	• sudo ufw default allow outgoing
	• sudo ufw allow 2200/tcp
	• sudo ufw allow 80/tcp
	• sudo ufw allow 123/udp
	• sudo ufw enable
	• sudo ufw status

# Create an SSH key pair for grader using the ssh-keygen tool:
	• sudo su grader
	• cd
	• mkdir .ssh
	• chmod 700 .ssh
	• cd
	• touch .ssh/authorized_keys
	• chmod 600 .ssh/authorized_keys
	• Copy your public key, and then use the Linux cat command (cat>>.ssh/authorized_keys) to paste the public key into the .ssh/authorized keys file for the new user grader.
	• To exit cat, enter EOF (Ctrl-D)

# Configure the local timezone to UTC.

	• sudo dpkg-reconfigure tzdata
		select "None of the above"
		select "UTC"
		
# Install and configure Apache to serve a Python mod_wsgi application.

# Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser.

# References:
https://aws.amazon.com/premiumsupport/knowledge-center/new-user-accounts-linux-instance/
https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html#how-to-generate-your-own-key-and-import-it-to-aws
https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair
https://forums.aws.amazon.com/thread.jspa?threadID=160352
http://brianflove.com/2013/06/18/add-new-sudo-user-to-ec2-ubuntu/
