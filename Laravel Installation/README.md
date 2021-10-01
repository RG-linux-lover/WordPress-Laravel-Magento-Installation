## Step1 - First update your repo

```console
ritik@ritik:~$ sudo apt update
```

## Step2 - Install apache2 and start, enable apache service

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
ritik@ritik:~$ sudo apt install php7.4 libapache2-mod-php7.4 php7.4-mysql php7.4-gd php7.4-xml php7.4-mbstring php7.4-zip
```

## Step6 -  Install composer (required for managing dependencies, libraries for laravel)

```console
ritik@ritik:~$ curl -sS https://getcomposer.org/installer | php
ritik@ritik:~$ sudo mv composer.phar /usr/local/bin/composer
ritik@ritik:~$ sudo chmod +x /usr/local/bin/composer
```

## Step7 - Install latest laravel version

```console
ritik@ritik:~$ cd /var/www/html/
ritik@ritik:~$ sudo git clone https://github.com/laravel/laravel.git 
```

or to install particular version [Click here](https://github.com/laravel/laravel/releases)

```console
ritik@ritik:~$ wget https://github.com/laravel/laravel/archive/refs/tags/v8.6.2.tar.gz
```

## Step8 -  Give proper permissions

```console
ritik@ritik:~$ sudo chown -R yourusername:yourusername /var/www/html/
ritik@ritik:~$ chmod -R 755 /var/www/html/laravel
ritik@ritik:~$ chmod -R 775 /var/www/html/laravel/storage
```

## Step9 - Run composer install to downloads and installs all the libraries and dependencies outlined in laravel dir

```console
ritik@ritik:~$ cd /var/www/html/laravel
ritik@ritik:~$ composer install
```

## Step10 - Move .env.example file to .env (It is a environment file to define things such as database connection settings, debug options, application URL, among other items that may vary depending on which environment the application is running.)
 
:warning: The environment configuration file contains sensitive information about your server, including database credentials and security keys. For that reason, you should never share this file publicly.

```console 
ritik@ritik:~$ mv .env.example .env
```

## Step11 - Now generate base64 random number encryption key, which used by the illuminate encrypter service.

```console
ritik@ritik:~$ php artisan key:generate 
```

## Step12 - Check the app_key get base64 ency. or not and you can also change the APP_NAME with the name of your application and APP_URL to the URL you need to access your Laravel application.

```console
ritik@ritik:~$ vi .env
		APP_NAME=Laravel
		APP_ENV=local
		APP_KEY=base64:HFdS7c9rhDp+AeHiu2232OLBPu232322/1gfFWEpoAk=
		APP_DEBUG=true
		APP_URL=http://localhost
```

## Step13 - Create user and database for laravel

```console
ritik@ritik:~$ sudo mysql -u root -p
		mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new-password';
		mysql> CREATE DATABASE laravel;
		mysql> CREATE USER 'laravel'@'localhost' IDENTIFIED BY 'User@123456';
		mysql> GRANT ALL PRIVILEGES ON *.* TO 'laravel'@'localhost' WITH GRANT OPTION;
		mysql> exit;
```

## Step14 Now edit the .env file and update database settings

```console
ritik@ritik:~$ vim .env
	        	DB_CONNECTION=mysql
	 	        DB_HOST=127.0.0.1
         		DB_PORT=3306
	        	DB_DATABASE=laravel
	        	DB_USERNAME=laravel
		        DB_PASSWORD=User@123456
```

## Step15 - Move Server.php to index.php

```console
ritik@ritik:~$ mv server.php index.php 
```

## Step16 - Restart apache service

```console
ritik@ritik:~$ sudo systemctl restart apache2
```

## Step17 - Access the laravel by the url 

```console
http://serverip-or-domainname/laravel/
```
