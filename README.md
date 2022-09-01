# VPS-Configure Ubuntu 20.04

- [VPS-Configure Ubuntu 20.04](#vps-configure-ubuntu-2004)
  - [Create Swap Memory](#create-swap-memory)
    - [Activate the swap partition permanently](#activate-the-swap-partition-permanently)
  - [Domain Add to Apache](#domain-add-to-apache)
  - [DATABASES](#databases)
    - [Create Database](#create-database)
    - [Create Databse User](#create-databse-user)
    - [Database Backup & Restore](#database-backup--restore)
    - [Database Auto Backup](#database-auto-backup)
  - [Email Setup](#email-setup)

## Create Swap Memory
To check swap memory:
```
free -m
```
Now, let’s create 4GB swap file:
```
sudo dd if=/dev/zero of=/swap count=4096 bs=1MiB
```
Now change the edit permissions:
```
sudo chmod 600 /swap
```
and make the swap file usable:
```
sudo mkswap /swap
```
Now enable the swap partition:
```
sudo swapon /swap
```
The swap partition is created, and it’s running correctly. But, the partition will not automatically be activated if you reboot the system.

### Activate the swap partition permanently
To make the swap partition automatically mounted after each reboot, execute the following command to add it to your system’s fstab config.
```
sudo echo "/swap swap swap sw 0 0" >> /etc/fstab
```
To check the current swappiness value:
```
cat /proc/sys/vm/swappiness
```
We can check the swappiness value with another command:
```
sysctl vm.swappiness
```
Change the swappiness value:
```
sudo sysctl vm.swappiness=1
```
To make the value permanent, edit the sysctl.conf file and change the swappiness value to 1 there.
```
sudo nano /etc/sysctl.conf
```

## Domain Add to Apache
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

## DATABASES
We can access database via command without phpmyadmin.
### Create Database
To enter mysql, type:
```
mysql
```
To create a database:
```
CREATE DATABASE database_name;
```

### Create Databse User
Create User:
```
CREATE USER 'user_name'@'localhost' IDENTIFIED BY 'password';
```
Access to specific Database:
```
GRANT ALL PRIVILEGES ON database_name.* TO 'user_name'@'localhost';
```
Access to All databases:
```
GRANT ALL PRIVILEGES ON * . * TO 'akgroup'@'localhost';
```
Flush Access:
```
FLUSH PRIVILEGES;
```

### Database Backup & Restore
To Backup
```
mysqldump -u akgroup --password=EzfowYLVqmtetsJP akfood | gzip > /home/akgroup/backup/backup_$(date +%F.%H%M%S)_akfood.sql.gz
```
To Restore
```
mysqldump -u akgroup --password=EzfowYLVqmtetsJP akfood | gzip < /home/akgroup/backup/backup_$(date +%F.%H%M%S)_akfood.sql.gz
```

### Database Auto Backup
Open Crontab by Nano:
``
sudo crontab -e
``
Add this line:
```
mysqldump -u akgroup --password=EzfowYLVqmtetsJP akfood | gzip > /home/akgroup/backup/backup_$(date +%F.%H%M%S)_akfood.sql.gz
```

## Email Setup
To check Ubuntu Version
```
lsb_release -a
```

## Mysql Command
```
SHOW DATABASES;
```
```
SELECT User, Host FROM mysql.user;
```
