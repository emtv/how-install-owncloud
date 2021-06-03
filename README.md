
# Quick guide - Install OwnCloud

## Step 1 - Config droplet on DigitalOcean

### Pick a Stack
I'm suggets LAMP (Linux Apache Mysql and PHP)

### Enter with SSH
Login with ssh in your droplet and run:

`sudo apt update`

`sudo apt upgrade`

## Step 2 - Download and install OwnCloud

You can use this src:

`wget https://download.owncloud.org/community/owncloud-complete-20200731.zip`

After unzip

`sudo unzip owncloud-complete-20200731.zip -d /var/www/html/`

You can install 'unzip' with:

`sudo apt-get install unzip`


### Folder permisions 
Don't forget Apache permisions

`sudo chown -R www-data:www-data /var/www/html/owncloud/`

`sudo chown -R 755 /var/www/html/owncloud/`


## Step 3 - Config your new domain
1. Go to networking configuration in you provider domain center.
2. Add new domain in you config.
3. Add new register A  (you new domain or subdomain)
4. Add new register Cname (wwww)

### Config your new domain on APACHE with virtualhost
You can view my [slide when explain how use virtual hosts](https://slides.com/edgardotupino/virtualhost)

#### Create a new register on APACHE

`sudo ln -s /etc/apache2/sites-available/owncloud.conf /etc/apache2/sites-enabled/owncloud.conf`

`sudo vim /etc/apache2/sites-available/owncloud.conf`

#### Add this config file inside
```
<VirtualHost *:80>
    ServerAdmin admin@yourdomain.com
    DocumentRoot /var/www/html/owncloud/
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    
    <Directory /var/www/html/owncloud/>
        Options FollowSymLinks
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
    
    ErrorLog /var/log/apache2/your-domain.com-error_log
    CustomLog /var/log/apache2/your-domain.com-access_log common
</VirtualHost>
```

#### Then you need reload the service 
Just follow this lines

`sudo a2ensite owncloud.conf`

`sudo a2enmod rewrite`

`sudo systemctl restart apache2`

## Step 4 - Create a database with MYSQL

Dont forget check the password. In DigitalOcean you can find it in 'cat ~/.digitalocean_password'. Then config your database.

```
CREATE DATABASE ownclouddb;
CREATE USER 'ownclouduser'@'localhost' IDENTIFIED BY 'YOURPASSWORD';
GRANT ALL ON ownclouddb.* TO 'ownclouduser'@'localhost';
FLUSH PRIVILEGES;
exit
```

## Step 5 - Install SSL
This example run with Let's Encrypt, you can pick other if you want. 

`sudo certbot --apache --agree-tos --redirect --staple-ocsp --email user@email.com -d domain.com`

`sudo certbot --apache -d domain.com -d`

## Step 6 -Go to domain
Access in the new domain and follow the install instructions.

## Common troubles

For example: module PHP zip not install

`sudo apt-get install php-zip`

`sudo apt-get install php-dom`

`sudo apt-get install php-xml`

`sudo apt-get install php-intl`

`sudo apt-get install php-mbstring`

`sudo apt-get install php7.4.curl -y`

`sudo apt-cache search php7.4-*`

## Don't forget reload APACHE

`sudo service apache2 reload`
