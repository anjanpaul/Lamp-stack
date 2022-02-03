# Introduction
A “LAMP” stack is a group of open source software that is typically installed together in order to enable a server to host dynamic websites and web apps written in PHP. This term is an acronym which represents the Linux operating system, with the Apache web server. The site data is stored in a MySQL database, and dynamic content is processed by PHP.

In this guide, you’ll set up a LAMP stack on an Ubuntu 20.04 server.

# Apache

## Step 1 — Installing Apache and Updating the Firewall

The Apache web server is among the most popular web servers in the world. It’s well documented, has an active community of users, and has been in wide use for much of the history of the web, which makes it a great choice for hosting a website.

Start by updating the package manager cache. If this is the first time you’re using sudo within this session, you’ll be prompted to provide your user’s password to confirm you have the right privileges to manage system packages with apt.



```
sudo apt update

```
Then, install Apache with:

```
sudo apt install apache2

```

You’ll also be prompted to confirm Apache’s installation by pressing Y, then ENTER.

Once the installation is finished, you’ll need to adjust your firewall settings to allow HTTP traffic. UFW has different application profiles that you can leverage for accomplishing that. To list all currently available UFW application profiles, you can run:

```
sudo ufw app list

```

You’ll see output like this:

![alt text]()

You can check which port bind using this command

```
netstat -tulnp

```

You’ll see output like this:

![alt text]()

This will give you two or three lines back. They are all correct addresses, but your computer may only be able to use one of them, so feel free to try each one.

An alternative method is to use the curl utility to contact an outside party to tell you how it sees your server. This is done by asking a specific server what your IP address is:

```
curl http://icanhazip.com

```

![alt text]()


# mysql

## Step 2 — Installing MySQL

Again, use apt to acquire and install this software:

```
sudo apt install mysql-server

```
When the installation is finished, it’s recommended that you run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to your database system. Start the interactive script by running:

```
sudo mysql_secure_installation

```
Edit mysqld.cnf file for ip bindings

```
vim /etc/mysql/mysql.conf.d/mysqld.cnf

```
bind-address = 127.0.0.1 to 0.0.0.0
mysqlx-bind-address = 127.0.0.1 to 0.0.0.0

after edit mysqld.cnf please restart mysql.service

```
sudo systemctl restart mysql.service

```

## Step 3 — Installing PHP

You have Apache installed to serve your content and MySQL installed to store and manage your data. PHP is the component of our setup that will process code to display dynamic content to the final user. In addition to the php package, you’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. You’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

To install these packages, run:

```
sudo apt install php libapache2-mod-php php-mysql

```

Once the installation is finished, you can run the following command to confirm your PHP version:

```
php -v

```