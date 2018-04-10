# FSND_Linux_Server
This project is linked to the Configuring Linux Web Servers course, which teaches you to secure and set up a Linux server. By the end of this project, you will have one of your web applications running live on a secure web server.

• Public IP: 34.205.53.16

• SSH Port: 2200

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

# Software Installed:
• apache2: Linux Web Server

• mod-wsgi: apache module for hosting python applications

• python-setuptools: For easily downloading, updating, developing python tools

• pip: Python package manager

• Flask: Python Web Development framework

• SQLAlchemy: SQL Object Relational mapper for python
• Postgresql: Object relational DBMS

• httplib2: HTTP client library for python

• requests: Another python HTTP library

• oauth2client: Authentication API for secure Facebook and Google Plus login.

• psychopg2: postgresql adapter for python

# Login to server:
Download the default AWS keypair

Login as ubuntu by running ssh ubuntu@34.205.53.16 -p 22 -i ~/.ssh/LightsailDefaultPrivateKey-us-east-1.pem
	
# Create a new user account named grader:
	• sudo adduser grader 
		Create password for grader
		Enter Full Name (I chose "Udacity Linux Grader")
		You can leave all other fields blank
		
# Give grader the permission to sudo:
	• sudo visudo
		add grader ALL=(ALL) ALL
		
# Update All currently installed packages:
	• sudo apt-get update
	• sudo apt-get upgrade 
		
# Change the SSH port from 22 to 2200:
	• sudo nano /etc/ssh/sshd_config 
		Change port 22 to 2200
		Change PermitRootLogin from without-password to no
		Change PasswordAuthentication from no to yes
		Add the following to the end of the file: 
			AllowUsers grader

# Create an SSH key pair for grader using the ssh-keygen tool:
	While in the terminal of you local machine:
		• ssh-keygen
		• copy public key
	Now, go to your lightsail terminal and:
		• sudo su - grader
		• sudo mkdir .ssh
		• sudo touch .ssh/authorized_keys
		• sudo nano .ssh/authorized_keys and paste your public key into the authorized_keys file
		• sudo chmod 700 .ssh
		• sudo chmod 644 .ssh/authorized_keys
	
# Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123):
	• sudo ufw default deny incoming 
	• sudo ufw default allow outgoing
	• sudo ufw allow 2200/tcp
	• sudo ufw allow 80/tcp
	• sudo ufw allow 123/udp
	• sudo ufw enable
	• sudo service ssh restart
	
# Configure the local timezone to UTC.
	• sudo dpkg-reconfigure tzdata
	• select "None of the above"
	• select "UTC"
	• sudo service ssh restart
	
		
Now I can login remotely as grader by running 
	• ssh grader@34.205.53.16 -p 2200 -i ~/.ssh/udacity_keypair

(Below steps are not working to get app up and running)
# Install and configure Apache to serve a Python mod_wsgi application:	
	• sudo apt-get install apache2
	• sudo apt-get install python-setuptools
	• sudo apt-get install libapache2-mod-wsgi
	• sudo service apache2 restart
	(now you can go to your ip address in your browser and see an apache2 message)
	
	• sudo apt-get install postgresql
	• sudo su - postgres
	• psql
	• postgres=# CREATE USER catalog WITH PASSWORD 'password';
	• postgres=# ALTER USER catalog CREATEDB;
	• postgres=# CREATE DATABASE catlog WITH OWNER catalog;;
	• postgres=# GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;
	• \q
	• exit
	
# Set it up in your server so that it functions correctly when visiting your server’s IP address in a browser.
	• sudo apt-get install git 
	• cd /var/www
	• sudo mkdir FlaskApp
	• cd FlaskApp
	• sudo chmod 777 /var/www/FlaskApp
	• git clone https://github.com/vrcarpentier/FSND_Catalog.git
	• sudo mv ./FSND_Catalog ./FlaskApp
	• cd FlaskApp
	• cd catalog
	• sudo mv webserver.py __init__.py
		from flask import Flask

      		app = Flask(__name__)
		
		@app.route('/')
		def homepage():
		    return  "VC808G Test Page"
		
		if __name__ == "__main__":
        	    app.run()
	• sudo nano database_setup.py and change engine = create_engine('sqlite:///catalog.db') to engine = create_engine('postgresql://catalog:password@localhost/catalog')
	• sudo nano lotsofmenus.py and change engine = create_engine('sqlite:///catalog.db') to engine = create_engine('postgresql://catalog:password@localhost/catalog').
	• sudo apt-get install python-pip
	sudo pip install virtualenv
	sudo virtualenv venv
	source venv/bin/activate
	sudo pip isntall Flask
	deactivate
	sudo nano /etc/apache2/sites-available/FlaskApp.conf
		<VirtualHost *:80>
			ServerName lotsofmenus.py
			ServerAdmin vrcarpentier@gmail.com
			WSGIScriptAlias / /var/www/FlaskApp/flaskapp.wsgi
			<Directory /var/www/FlaskApp/FlaskApp/>
				Order allow,deny
				Allow from all
			</Directory>
			Alias /static /var/www/FlaskApp/FlaskApp/static
			<Directory /var/www/FlaskApp/FlaskApp/static/>
				Order allow,deny
				Allow from all
			</Directory>
			ErrorLog ${APACHE_LOG_DIR}/error.log
			LogLevel warn
			CustomLog ${APACHE_LOG_DIR}/access.log combined
		</VirtualHost>
	sudo a2ensite FlaskApp
	service apache2 reload
	cd /var/www/flaskapp
	sudo nano flaskapp.wsgi
		#! /usr/bing/python

		import sys
		import logging

		logging.basicConfig(stream=sys.stderr)
		sys.path.insert(0, "/var/www/FlaskApp/")

		from FlaskApp import app as application
		application.secret_key = "super_secret_key"
		
	sudo service apache2 restart
	• sudo pip install sqlalchemy flask-sqlalchemy psycopg2 bleach requests
	• sudo pip install flask packaging oauth2client redis passlib flask-httpauth
	• sudo apt-get -qqy install postgresql python-psycopg2
	• sudo python database_setup.py
	• sudo pip install lotsofmenus.py	

# References:
https://aws.amazon.com/premiumsupport/knowledge-center/new-user-accounts-linux-instance/
https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html#how-to-generate-your-own-key-and-import-it-to-aws
https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair
https://forums.aws.amazon.com/thread.jspa?threadID=160352
http://brianflove.com/2013/06/18/add-new-sudo-user-to-ec2-ubuntu/
