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

## Step5 - Create user for magento and remember its password and give it permission to create database

```console
ritik@ritik:~$ sudo mysql -u root -p
               mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'new-password';
               mysql> CREATE USER 'magento'@'localhost' IDENTIFIED BY 'Password@123';
               mysql> GRANT ALL PRIVILEGES ON *.* TO 'magento'@'localhost' WITH GRANT OPTION;
	       mysql> CREATE DATABASE magento;
               mysql> exit;
```

## Step5 -  Install required php7.4 packages 

```console 
ritik@ritik:~$ sudo apt install php7.4 libapache2-mod-php7.4 php7.4-mysql php7.4-bcmath php7.4-intl php7.4-soap php7.4-zip php7.4-gd php7.4-json php7.4-curl php7.4-cli php7.4-xml php7.4-xmlrpc php7.4-gmp php7.4-common php7.4-mbstring
```

## Step6 -  Install composer (required for managing dependencies, libraries for laravel)

```console
ritik@ritik:~$ curl -sS https://getcomposer.org/installer | php
ritik@ritik:~$ sudo mv composer.phar /usr/local/bin/composer
ritik@ritik:~$ sudo chmod +x /usr/local/bin/composer
```

## Step7 - Download the latest version of the Magento from [GitHub](https://github.com/magento/magento2) or from [Official Website](https://www.mageplaza.com/download-magento/)

```console
ritik@ritik:~$ cd /var/www/html
ritik@ritik:~$ sudo wget https://github.com/magento/magento2/archive/2.3.zip
ritik@ritik:~$ sudo apt install unzip
ritik@ritik:~$ sudo unzip 2.3.zip
ritik@ritik:~$ sudo mv magento2-2.3/ magento
```

## Step8 - Give proper permission to magento files so that magento application can run

```console
ritik@ritik:~$ sudo chmod -R 777 var/ pub/static generated/
ritik@ritik:~$ sudo chmod -R 777 /var/www/html/magento/app/etc
ritik@ritik:~$ sudo chmod -R 777 /var/www/html/magento/pub/media/
```

## Step9 - Run composer install to downloads and installs all the libraries and dependencies outlined in Magento dir

```console
ritik@ritik:~$ cd /var/www/html/magento
ritik@ritik:~$ sudo composer install
```

## Step10 - 

:information_source: Edit the information to match your requirements and the configuration of your system:

* base-url - The location (URL) of your store. In this example, the store is installed on the localhost in the magento2.4 sub-directory.
* db-host - If Magento is on the same server as your database, use localhost. If you are using a separate database server, enter the hostname of that server.
* db-name - The name of the MySQL database created earlier.
* db-user - Enter the username of your MySQL user.
* db-password -  The password for your MySQL user.
* admin-firstname and admin-lastname - Set the full name for your Magento admin user.
* admin-email - Define a contact email for system notifications and password resets.
* admin-user / admin-password - Create the login credentials for the Magento Admin control panel.
* language - Defines the default language for your store.
* currency - Sets the base currency for your store.
* timezone - Regulates the default time zone for Magento.

```console
ritik@ritik:~$ php bin/magento setup:install --base-url="http://172.25.250.198/magento/" --db-host="localhost" --db-name="magento" --db-user="magento" --db-password="Password@123" --admin-firstname="admin" --admin-lastname="admin" --admin-email="admin@admin.com" --admin-user="admin" --admin-password="Password@123" --language="en_US" --currency="INR" --timezone="America/Chicago" --backend-frontname="admin"
```

## Step11 - To run magento properly allow it 

```console
ritik@ritik:~$ sudo vim /etc/apache2/apache2.conf
		<Directory /var/www/>
       			 Options Indexes FollowSymLinks
       		         AllowOverride All
       			 Require all granted
		</Directory> 
```

## Step12 - Enable mbstring module and restart apache service

```console
ritik@ritik:~$ sudo phpenmod mbstring
ritik@ritik:~$ sudo a2enmod rewrite
ritik@ritik:~$ sudo systemctl restart apache2
```

## Step13 - Go to browser and type the url as show below

```console
         your-ip/magento/admin
```

![Magento admin page](https://docs.magento.com/user-guide/stores/assets/admin-login.png)

