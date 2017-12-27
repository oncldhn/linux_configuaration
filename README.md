# Linux Server Configuration
Udacity Full Stack Web Developer Nanodegree - Linux Server Configuration

IP Address: 18.195.249.116
SSH Port: 2200
user: grader, password: udacity
url : http://18.195.249.116

### Amazon Lightsail
1. Get an account on Amazon Lightsail and create an instance
2. Get static ip

### Grader user
1. Create the "grader" user. Give sudo permissions
2. Create a key on your machine and copy public key to your server
3. Copy public key to /home/grader/.ssh/authorized_keys
4. Disable root login by changing PermitRootLogin property to no in /etc/ssh/sshd_config

### Firewall Setup
1. change ssh port to 2200 in /etc/ssh/sshd_config
2. configure ufw by
  * `sudo ufw allow 2200/tcp`
  * `sudo ufw allow 80/tcp`
  * `sudo ufw allow 123/ud`
3. run sudo ufw enable

### Item catalog Setup
1. git clone the project under /var/www
2. Create a itemcatalog.wsgi file, then add this inside:

 activate_this = '/var/www/itemcatalog/venv/bin/activate_this.py'
 execfile(activate_this, dict(__file__=activate_this))

 import sys
 import logging
 logging.basicConfig(stream=sys.stderr)
 sys.path.insert(0, "/var/www/")

 from itemcatalog import app as application
 application.secret_key = 'super_secret_key'

3. Rename itemcatalog.py to __init__.py
4. Install virtual environment
5. Install all dependencies
6. Refactor filepaths in code
7. Change google sign in urls
8.Configure and enable a new virtual host
   * Run this: `sudo nano /etc/apache2/sites-available/catalog.conf`
   * Paste this code:
      ```
		<VirtualHost *:80>
			ServerName 18.195.249.116
			ServerAlias ip-172-26-0-155.us-frankfurt-1.compute.amazonaws.com
			ServerAdmin admin@18.195.249.116
			WSGIDaemonProcess itemcatalog python-path=/var/www/itemcatalog:/var/www/itemcatalog/venv/lib/python2.7/site-packages
			WSGIProcessGroup itemcatalog
			WSGIScriptAlias / /var/www/itemcatalog.wsgi
			<Directory /var/www/itemcatalog/>
				Order allow,deny
				Allow from all
			</Directory>
			Alias /static /var/www/itemcatalog/static
			<Directory /var/www/itemcatalog/static/>
				Order allow,deny
				Allow from all
			</Directory>
			ErrorLog ${APACHE_LOG_DIR}/error.log
			LogLevel warn
			CustomLog ${APACHE_LOG_DIR}/access.log combined
		</VirtualHost>
      ```
   * Enable the virtual host `sudo a2ensite catalog`
9. Restart Apache
      * `sudo service apache2 restart`

### Resources
https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps
https://github.com/iliketomatoes/linux_server_configuration
