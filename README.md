# WordPress Setup 
This is a Simple wordpress setup on Centos 7 


# Install prerequisites 

* Configure Apache 
- sudo yum install nano -y
- sudo yum install httpd mariadb phpphp-zip php-fileinfo  php-opcache php-curl mariadb-server php php-common php-mysql php-gd php-xml php-mbstring php-mcrypt php-xmlrpc unzip wget -y
- sudo systemctl start httpd
- sudo systemctl start mariadb
- sudo systemctl enable httpd
- sudo systemctl enable mariadb

* Configure Database
- sudo mysql_secure_installation
- mysql -u root -p
- CREATE DATABASE wordpress;
- GRANT ALL PRIVILEGES on wordpress.* to 'mrichardson'@'localhost' identified by 'password';
- UPDATE mysql.user SET Password=password('mrichardson') WHERE User='mrichardson';
- flush privileges;
- exit#

* Configure apache/wordpress 
- sudo mkdir -p /var/www/your-domainhere.com/html
- sudo mkdir -p /var/www/your-domainhere.com/log
- sudo nano /var/www/your-domainhere.com/html/index.html
- sudo chown -R apache:apache /var/www/your-domainhere.com/html
- sudo chown -R apache: /var/www/
- sudo mkdir /etc/httpd/sites-available /etc/httpd/sites-enabled
- sudo nano /etc/httpd/conf/httpd.conf
- (change the DocumentRoot location from /var/www/html/ to /var/www/
- Add to the end of the file:
- IncludeOptional sites-enabled/*.conf
- sudo nano /etc/httpd/sites-available/your-domainhere.com.conf

```
#Add the following contents to the file:
<VirtualHost *:80>
    ServerName your-domainhere.com
    ServerAlias devops.your-domainhere.com
    ServerAdmin webmaster@your-domainhere.com
    DocumentRoot /var/www/your-domainhere.com/html

    <Directory /var/www/your-domainhere.com/html>
        Options -Indexes +FollowSymLinks
        AllowOverride All
    </Directory>

    ErrorLog /var/log/httpd/your-domainhere.com-error.log
    CustomLog /var/log/httpd/your-domainhere.com-access.log combined
</VirtualHost>
```

- sudo ln -s /etc/httpd/sites-available/your-domainhere.com.conf /etc/httpd/sites-enabled/your-domainhere.com.conf
- sudo systemctl restart httpd
- sudo yum install -y epel-release yum-utils && sudo yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm
- sudo yum-config-manager --enable remi-php73
- sudo yum install -y php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysqlnd
- php -v
- sudo systemctl restart httpd
- sudo wget http://wordpress.org/latest.tar.gz
- sudo tar -xzvf latest.tar.gz
- sudo cp -avr wordpress/* /var/www/your-domainhere.com/html/
- sudo mkdir -p /var/www/your-domainhere.com/html/wp-content/uploads
- sudo chown -R apache:apache /var/www/your-domainhere.com/html/
- sudo chmod -R 755 /var/www/your-domainhere.com/html/

```
- options:
=1 Can to go the web browser and enter http:<IP ADDRESS>/wp-activate.php and follow the prompts.

=2 cd /var/www/your-domainhere.com/html/
sudo mv wp-config-sample.php wp-config.php
sudo nano wp-config.php


[Main issues along the way]
Incomplete documentation from posts.
setting up the new .conf file correctly (apachectl configtest we OK) php-opcache
```
