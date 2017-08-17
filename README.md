# Linux-Server-Configuration
---
Configuring a Linux Server to Host a Web Application

## Server
---
* IP Address: 34.227.172.211

* Accessible SSH port: 2200

## Tasks
---
1. Create a user `grader` using command: `sudo adduser thenameofuser`

2. Give user `grader` `sudo` access using command: `usermod -aG sudo grader`

3. Update all currently installed packages by using:
..* First update your package source list `sudo apt-get update`
..* Now we know what all software availabe, Let's upgrade installed packages `sudo apt-get upgrade`
..* Run `reboot` to restart the machine.

4. Generate Key pair on our local machine using `ssh-keygen` and run it.

5. Installing a Publick key:
* Login to server as grader
* Create `.ssh` using `mkdir .ssh`
* Create a new file within this `.ssh` directory called `authorized_keys` `touch .ssh/authorized_keys`
* `vim .ssh/authorized_keys`
* `chmod 700 .ssh`
* `chmod 644 .ssh/authorized_keys`
* `service ssh restart`
* You can user ssh to login new user `ssh -i privatekeyfile grader -p 2200 grader@34.227.172.211`

6. Disable root login
* `sudo nano /etc/ssh/sshd_config`
* Uncomment `PasswordAuthentication no`
* Change `PermitRootLogin without-password` to `PermitRootLogin no`
* `service ssh restart`

7. Steps to configuring Ports in UFW
* Allow all TCP connection through 2200 `sudo ufw allow 2200/tcp`
* Allow basic http server by `sudo ufw allow 80/tcp` or `sudo ufw allw www`
* Allow all NTP connection through port 123 by `sudo ufw allow ntp`ss
* Enable firewall by `sudo ufw enable`
* You can check status by `sudo ufw status`

8. Install Apache to serve a Python mod_wsgi application
* Install Apache by `sudo apt-get install apache2`
* Install mod_wsgi by `install apt-get install python-setuptools libapache2-mod-wsgi`
* Restart Apache by `sudo service apache2 restart`

9. Install and Configure PostgreSQL
* Install PostgreSQL by using `sudo apt-get install postgresql`
* Make sure remote connections are not allowe by checking `sudo vim /etc/postgresql/9.3/main/pg_hba.conf`
* Connect as user postgres by typing `sudo su - postgres`
* Type `psql` to bring up the PostgreSQL prompt
* Create a PostgreSQL user called `catalog`, type `CREATE USER catalog WITH PASSWORD 'your_password';`
* Create an empty database called catalog by typing `sudo -u postgres createdb -O catalog catalog`
* Give user `catalog` permission to catalog database `GRANT ALL PRIVILEGES ON DATABASE catalog TO catalog;`
* Check if the user created by typing `\du`
* Quit postgreSQL by typing `\q` and to exit `exit`

10. Install git and setup the project
* Install git by typing `sudo apt-get install git`
* Go to /var/www directory by typing `cd /var/www`
* Create the app directory by `mkdir catalog-project`
* Move to `catalog-project` directory by typing `cd catalog-project`
* Clone the project to the virtual machine by typing `git clone https://github.com/ccevik/psql-catalog psqlcatalog`
* Move to psqlcatalog by typing `cd psqlcatalog`
* Change file  name `project.py` to `__init__.py`
* Edit `database_setup.py` and `__init__.py` by changing
  `engine = create_engine('postgresql://catalog:catalog@localhost/catalog/')`
* Install pip by typing `sudo apt-get install python-pip`
* Type all dependencies in `requirements.txt`
* Use pip to install all dependencies `sudo pip install -r requirement.txt`
* Install psycopg2 by typing `sudo apt-get -qqy install postgresql python-psycopg2`
* Create database schema by typing `sudo python database_setup.py`

11. Configure and Enable a new Virtual host
* Move to `sites-available` directory by typing `/etc/apache2/sites-available`
* Create catalog.conf file `sudo nano catalog.conf`
* Add the following to `catalog.conf` to configure the virtual host
  `<VirtualHost *:80>

                ServerAdmin grader@34.227.172.211
                WSGIScriptAlias / /var/www/catalog-project/catalog.wsgi
                <Directory /var/www/catalog-project/>
                        Order allow,deny
                        Allow from all
                </Directory>
                Alias /static /var/www/catalog-project/psqlcatalog/static
                <Directory /var/www/catalog-project/psqlcatalog/static/>
                        Order allow,deny
                        Allow from all
                </Directory>
                ErrorLog ${APACHE_LOG_DIR}/error.log
                LogLevel warn
                CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

`
* Enable the virtual host by typing `sudo a2ensite catalog.conf`

12. Create the .wsgi file
* Move to catalog-project by typing `cd /var/www/catalog-project`
* Create `catalog.wsgi` file `sudo nano catalog.wsgi` and add the following:
```
#!/usr/bin/python
import sys
import logging
logging.basicConfig(stream=sys.stderr)
sys.path.insert(0,"/var/www/catalog-project/")
from psqlcatalog import app as application
application.secret_key = 'super_secret_key'
```

13. Restart Apache
* `sudo service apache2 restart`

14. `sudo python __init__.py` and visit `http://34.227.172.211`

## References
[Engine Configuration](http://docs.sqlalchemy.org/en/latest/core/engines.html)
[Deploy a Flask Application](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
[Add and Deleter Users](https://wwrw.digitalocean.com/community/tutorials/how-to-add-and-delete-users-on-an-ubuntu-14-04-vps)
[Secure PostgreSQL](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps)
