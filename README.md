# Linux-Server-Configuration

Configuring a Linux Server to Host a Web Application

## Server

* IP Address: 34.227.172.211

* Accessible SSH port: 2200

## Tasks

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

