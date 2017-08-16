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
..* Login to server as grader
..* Create `.ssh` using `mkdir .ssh`
..* Create a new file within this `.ssh` directory called `authorized_keys` `touch .ssh/authorized_keys`
..* `vim .ssh/authorized_keys`
..* `chmod 700 .ssh`
..* `chmod 644 .ssh/authorized_keys`
..* `service ssh restart`
..* You can user ssh to login new user `ssh -i privatekeyfile grader -p 2200 grader@34.227.172.211`
6. 
