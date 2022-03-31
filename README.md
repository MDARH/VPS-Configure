# VPS-Configure Ubuntu 20.04

- [VPS-Configure Ubuntu 20.04](#vps-configure-ubuntu-2004)
  - [Create Swam Memory](#create-swam-memory)
  - [Domain Add to Apache](#domain-add-to-apache)
  - [Create Database](#create-database)
  - [Create Databse User](#create-databse-user)
  - [Database Backup & Restore](#database-backup--restore)
  - [Database Auto Backup](#database-auto-backup)


## Create Swam Memory
At first, you need to check if your system already has a swap space. Access your server with an SSH client and write the following command.
``
free -m
``
## Domain Add to Apache
## Create Database
## Create Databse User
## Database Backup & Restore
## Database Auto Backup

sudo chown -R $USER:$USER /home/akgroup
sudo chmod -R 755 /home/akgroup

sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/akhalalfood.com.conf

sudo nano /etc/apache2/sites-available/akhalalfood.com.conf

<VirtualHost *:80>
    ServerAdmin akgroupjp20@gmail.com
    ServerName akhalalfood.com
    ServerAlias www.akhalalfood.com
    DocumentRoot /home/akgroup/akhalalfood.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

sudo a2ensite akhalalfood.com.conf
sudo a2dissite 000-default.conf

sudo systemctl restart apache2
sudo systemctl status apache2


CREATE USER 'akgroup'@'localhost' IDENTIFIED BY 'mbol (# $ ! % & etc...)1';

GRANT ALL PRIVILEGES ON akfood.* TO 'akgroup'@'localhost';

GRANT ALL PRIVILEGES ON * . * TO 'akgroup'@'localhost';
FLUSH PRIVILEGES;

mysql -u akgroup -p akfood < /home/akgroup/akhalalfood.com/20222803_backup.sql
