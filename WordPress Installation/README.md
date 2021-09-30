## Step1 - First update your repo

```console
ritik@ritik:~$ sudo apt-get update
```

## Step2 - Install apache2 and start and enable apache service

```console
ritik@ritik:~$ sudo apt-get install apache2 apache2-utils
ritik@ritik:~$ sudo systemctl start apache2
ritik@ritik:~$ sudo systemctl enable apache2
```

## Step3 - Once you’ve started Apache, you then need to allow HTTP traffic on your UFW firewall

```console
ritik@ritik:~$ sudo ufw allow in "Apache"
ritik@ritik:~$ sudo ufw status
```

#### To test whether the Apache server is running, open your web browser and enter the following URL in the address bar
```bash
http://server_address
```

## Step4 - Now, Install mysql-server and set password for root

```console
ritik@ritik:~$ sudo apt-get install mysql-server
ritik@ritik:~$ sudo mysql_secure_installation
```

## Step5 -  Install required php7.4 packages 

```console 
ritik@ritik:~$ sudo apt install php7.4 libapache2-mod-php7.4 php7.4-mysql php7.4-curl php7.4-gd php7.4-xml php7.4-mbstring php7.4-xmlrpc php7.4-intl php7.4-soap php7.4-zip
```

## Step6 - Download the latest version of the WordPress from GitHub

```console
ritik@ritik:~$ cd /var/www/html
ritik@ritik:~$ sudo git clone https://github.com/WordPress/WordPress.git
```

## Step7 - Create user for wordpress and remember its password and give it permission to create database

```console
ritik@ritik:~$ sudo mysql -u root -p
               mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new-password';
               mysql> CREATE USER 'wordpress'@'localhost' IDENTIFIED BY 'Password@123';
	       mysql> GRANT ALL PRIVILEGES ON *.* TO 'wordpress'@'localhost' WITH GRANT OPTION;
	       mysql> exit;
```

## Step9 - Now Create DataBase for wordpress

```console
ritik@ritik:~$ sudo mysql -u wordpress -p
	       mysql> CREATE DATABASE wp;
	       mysql> exit;
```

## Step10 - Move sample file to activate it

```console
ritik@ritik:~$ cd /var/www/html/wordpress
ritik@ritik:~$ sudo mv wp-config-sample.php wp-config.php
```

## Step11 - Now set DataBase User, DataBase Name and DataBase Password in wp-config.php file 

```console
ritik@ritik:~$ sudo vim wp-config.php
                
                  define( 'DB_NAME', 'wp' );
		  define( 'DB_USER', 'wordpress' );
		  define( 'DB_PASSWORD', 'Password@123' );
```

## Step12 - Restart Apache and MySql service

```console
ritik@ritik:~$ sudo systemctl restart apache2
ritik@ritik:~$ sudo systemctl restart mysql
```

## Step13 - Go to Browser and type

```bash
   your-machine-ip/wordpress/wp-admin
```
![WordPress admin page](https://i2.wp.com/wordpress.org/support/files/2018/10/install-step5_v47.png?ssl=1)

## You can set below details on your WordPress installation Setup

```bash
Site Title = yoursitename
Username =  your-username
Password = StrongPassword
Email = your.email@wordpress.com
```




