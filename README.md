# FSND_Linux_Server
IP Address: 35.153.156.123

SSH Port: 2200

URL: 

# Instructions:
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


# Update All currently installed packages:

	• sudo apt-get update,
	• sudo apt-get upgrade, 
	• sudo apt-get autoremove, 
	• sudo apt-get install finger

# Change the SSH port from 22 to 2200:

	• sudo vi /etc/ssh/sshd_config
	• Change port 22 to 2200

# Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port    123):

	• sudo ufw status
	• sudo ufw default deny incoming
	• sudo ufw default allow outgoing
	• sudo ufw allow 2200/tcp
	• sudo ufw allow 80/tcp
	• sSudo ufw allow 123/udp
	• sudo ufw enable

# Create a new user account named grader:

	• sudo adduser grader

# Give grader the permission to sudo:

	• sudo nano /etc/sudoers.d/grader
	• type in grader ALL=(ALL:ALL) ALL save and quit

# Create an SSH key pair for grader using the ssh-keygen tool:

# Configure the local timezone to UTC.

	• sudo dpkg-reconfigure tzdata

# Install and configure Apache to serve a Python mod_wsgi application.

# Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser.
