# Linux Server Configuration

### Project Overview

>This project includes setting up the Linux Server, configuring database and then deploying Item Catalog Application on it and securing it with any      possible security threats.

* **Public IP Address**: `13.126.30.40`
* The cloud server is on <a href="https://amazonlightsail.com/">**Amazon Lightsail**</a>, Amazon Lightsail is the easiest way to launch and manage a virtual private server with AWS.
* To view the deployed Application on the cloud server following is the URL to the website.<br>
  ### <a href="http://ec2-13-126-30-40.ap-south-1.compute.amazonaws.com/">http://ec2-13-126-30-40.ap-south-1.compute.amazonaws.com/</a>
  
#### NOTE
>The above given domain name is converted on the given below link through the Public IP ADDRESS.<br>
<a href="http://www.nmonitoring.com/ip-to-domain-name.html?ip=13.126.30.40&pingsub=1&ln=en">Convert your IP</a>

### Project Execution

###### Connect as root on server and then Creating the new user grader and giving sudo access

```
$ sudo adduser grader
$ sudo touch /etc/sudoers.d/grader
$ sudo nano /etc/sudoer.d/grader
```
Now, In this file, write and save the following:<br>
```
grader ALL=(ALL) NOPASSWD: ALL
```

###### Allowing grader to login via generated Public Key

When connected as root on server. You will need the **Private Key** to log in, which you can find on this page of the **Lightsail website.**<br>
First login on your AWS account ang go to the following link:<br>
<a href="https://lightsail.aws.amazon.com/ls/webapp/account/keys">https://lightsail.aws.amazon.com/ls/webapp/account/keys</a><br>
Now under **SSH key pairs** select default option and then download the **Private Key** on your system.<br>
**Public Key** will be automatically uploaded on your cloud server, to view your Public Key run the following command on your cloud server:
```
cat /.ssh/authorized_keys
```

###### Connecting to the Cloud Server

* Paste the content(**Private Key**) given in the reviewer notes in a `Private-Key.pub` file
* Then open **CLI**(**GIT Bash**: recommended) in your system and run the following command<br>
```$ ssh grader@ip-address -p 2200 -i Private-Key.pub```

###### Updating the server with most recent softwares

* Run the following commands<br>
```
grader@ip-address:~$ sudo apt-get update
grader@ip-address:~$ sudo apt-get upgrade
```

###### Disabling `Root login`, Enforcing `key-based authentication` and changing `SSH port`

* Run the following commands:
```
grader@ip-address:~$ sudo nano /etc/ssh/sshd_config
```
* Find **PermitRootLogin** line and append it with **no**<br>
* Find the **PasswordAuthentication** line and append it with **no**<br>
* Find the **Port** line and change **22** to **2200**<br>
* Restart **SSH** service using:<br>
```
grader@ip-address:~$ sudo service ssh restart
```

###### Configuring local timezone to UTC

```
grader@ip-address:~$ sudo timedatectl set-timezone UTC
```

###### Configuring `UFW(Uncomplicated Fire Wall)`

```
$ sudo ufw default deny incoming
$ sudo ufw default allow outgoing
$ sudo ufw allow 2200/tcp
$ sudo ufw allow 80/tcp
$ sudo ufw allow 123/udp
$ sudo ufw enable
```

###### Install and Enable mod_wsgi
**WSGI** (Web Server Gateway Interface) is an interface between web servers and web apps for python. **Mod_wsgi** is an **Apache HTTP server mod** that enables Apache to serve **Flask applications**.

Open terminal and type the following command to install mod_wsgi:
```
grader@ip-address:~$ sudo apt-get install libapache2-mod-wsgi python-dev
```
To enable mod_wsgi, run the following command:
```
grader@ip-address:~$ sudo a2enmod wsgi 
```
###### Install and Configure PostgreSQL

* Install the required packages
```
  grader@ip-address:~$ sudo apt-get install postgresql postgresql-contrib libpq-dev python-dev
```
* Login as postgres User(Default user), and get into psql shell
```
grader@ip-address:~$ sudo su postgres
postgres@ip-address:~$ psql
```
* Create new User named catalog `CREATE USER catalog WITH PASSWORD 'heisenberg';`
* Create a new Database itemcatalog_database `CREATE DATABASE itemcatalog_database WITH OWNER catalog;`
* Connect to the Database `\c itemcatalog_database`
* Revoke all rights from public, and allow only catalog to perform transactions
```
itemcatalog_database=# REVOKE ALL ON SCHEMA public FROM public;
itemcatalog_database=# GRANT ALL ON SCHEMA public TO catalog;
```
###### Install Item Catalog App dependencies

* Install **Python** and **Virtual Environment**:
```
grader@ip-address:~$ sudo apt-get install python-pip
grader@ip-address:~$ sudo apt-get install python-virtualenv
```
* To create virtual Environment to install python modules run the following commands and activate virtual environment:
```
sudo virtualenv flask-env
source flask-env/bin/activate
```
* Now install the required modules:
```
(flask-env)grader@ip-address:~$ sudo pip install flask
(flask-env)grader@ip-address:~$ sudo pip install oauth2client sqlalchemy psycopg2 requests
```