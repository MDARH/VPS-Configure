# VPS-Configure Ubuntu 20.04

- [VPS-Configure Ubuntu 20.04](#vps-configure-ubuntu-2004)
  - [Create Swam Memory](#create-swam-memory)
    - [Activate the swap partition permanently](#activate-the-swap-partition-permanently)
    - [Changing the swappiness value](#changing-the-swappiness-value)
  - [Domain Add to Apache](#domain-add-to-apache)
  - [Create Database](#create-database)
  - [Create Databse User](#create-databse-user)
  - [Database Backup & Restore](#database-backup--restore)
  - [Database Auto Backup](#database-auto-backup)


## Create Swam Memory
At first, you need to check if your system already has a swap space. Access your server with an SSH client and write the following command.
```
free -m
```
Now, let’s create the swap file. I’m going to create a 4GB file. However, you may change it depending on your needs. The common practice is that the swap file size should be half to the physical memory. But, it should not be more than 4GB.
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
```
sudo echo "/swap swap swap sw 0 0" >> /etc/fstab
```
Check the current swappiness value
```
cat /proc/sys/vm/swappiness
```
We can check the swappiness value with another command, and that is:
```
sysctl vm.swappiness
```
### Changing the swappiness value
```
sudo sysctl vm.swappiness=1
```
To make the value permanent, edit the sysctl.conf file and change the swappiness value to 1 there.
```
sudo nano /etc/sysctl.conf
```

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
