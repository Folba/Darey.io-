# WEB SOLUTION WITH WORDPRESS

###  Step 1 — Prepare a Web Server

Lunch Ec2 webserver and attach 3 Volumes 

Use " lsblk " command to inspect what block devices are attached to the server

Use " df -h "command to see all mounts and free space on your server

Use gdisk utility to create a single partition on each of the 3 disks


<img width="1231" alt="gdisk" src="https://user-images.githubusercontent.com/97977311/151986236-43651239-538d-4334-8b3e-965d4d6537ee.png">


Use lsblk utility to view the newly configured partition on each of the 3 disks.

<img width="568" alt="lsblk webserver" src="https://user-images.githubusercontent.com/97977311/151988686-5d10ed3e-8b8a-49b0-ad4c-5a3650773db0.png">

Install lvm2 package using "sudo yum "install lvm2

<img width="1231" alt="install lvm2" src="https://user-images.githubusercontent.com/97977311/151989407-f0b621a4-914a-48b9-9292-ee20cd711873.png">

Use " pvcreate " utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM

Also list the Physical Volume  using sudo pvs
<img width="1231" alt="sudo pvs" src="https://user-images.githubusercontent.com/97977311/152007500-1b43a37a-68df-484b-809f-11540613705a.png">


sudo vgcreate webdata-vg /dev/xvdh1 /dev/xvdg1 /dev/xvdf1

<img width="1161" alt="vgcreate" src="https://user-images.githubusercontent.com/97977311/151990526-f85a882a-2651-438a-a02f-085d68892df8.png">

Use lvcreate utility to create 2 logical volumes. apps-lv and log-lv

sudo lvcreate -n apps-lv -L 14G webdata-vg

sudo lvcreate -n logs-lv -L 14G webdata-vg

<img width="1013" alt="sudo lvcreate" src="https://user-images.githubusercontent.com/97977311/152009056-541d437c-e9b1-4cd3-8967-92d4b32cb993.png">

Verify logical volume 

sudo lvs

<img width="1013" alt="sudo lvs" src="https://user-images.githubusercontent.com/97977311/152010116-e10060c6-749e-44b4-8947-a1b61b78a329.png">

Use mkfs.ext4 to format the logical volumes with ext4 filesystem

sudo mkfs -t ext4 /dev/webdata-vg/apps-lv

sudo mkfs -t ext4 /dev/webdata-vg/logs-lv

Create /var/www/html directory to store website files

sudo mkdir -p /var/www/html

Create /home/recovery/logs to store backup of log data

sudo mkdir -p /home/recovery/logs

Mount /var/www/html on apps-lv logical volume

sudo mount /dev/webdata-vg/apps-lv /var/www/html/

Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/log before mounting the file system.

sudo rsync -av /var/log/. /home/recovery/logs/

sudo mount /dev/webdata-vg/logs-lv /var/log

Restore backup files to /var/log

sudo rsync -av /home/recovery/logs/. /var/log

UPDATE THE `/ETC/FSTAB` FILE

The UUID of the device will be used to update the /etc/fstab file;

sudo blkid

<img width="1236" alt="lsblkid for fstab" src="https://user-images.githubusercontent.com/97977311/152056829-36bdd8b0-1ce7-4a09-8300-190fc1aa0d6e.png">

sudo vi /etc/fstab


<img width="1402" alt="fstab" src="https://user-images.githubusercontent.com/97977311/152057996-46cbf5e7-d861-4ab2-a949-54cbbdcd7992.png">

Test the configuration and reload the daemon

 sudo mount -a
 
 sudo systemctl daemon-reload
 
 df -h
 
 <img width="1109" alt="df -h" src="https://user-images.githubusercontent.com/97977311/152060724-b589ae13-40a2-4f94-934e-417300f441c4.png">
 
 ## Step 2 — Prepare the Database Server
 
Launch a second RedHat EC2 instance that will have a role – ‘DB Server’
 
Repeat the same steps as for the Web Server and mount it to /db directory

<img width="1017" alt="db-lv" src="https://user-images.githubusercontent.com/97977311/152062007-685d25f9-767d-4557-8026-116b41de16b8.png">



Update the repository

sudo yum -y update

Install wget, Apache and it’s dependencies

sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json

Start Apache

sudo systemctl enable httpd

sudo systemctl start httpd

#### To install PHP and it’s depemdencies

sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo yum install yum-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo yum module list php

sudo yum module reset php

sudo yum module enable php:remi-7.4

sudo yum install php php-opcache php-gd php-curl php-mysqlnd

sudo systemctl start php-fpm

sudo systemctl enable php-fpm

setsebool -P httpd_execmem 1

Restart Apache

sudo systemctl restart httpd

#### Download wordpress and copy wordpress to var/www/html

  mkdir wordpress
  
  cd   wordpress
  
  sudo wget http://wordpress.org/latest.tar.gz
  
  sudo tar xzvf latest.tar.gz
  
  sudo rm -rf latest.tar.gz
  
  cp wordpress/wp-config-sample.php wordpress/wp-config.php
  
  cp -R wordpress /var/www/html/
  
#### Configure SELinux Policies

  sudo chown -R apache:apache /var/www/html/wordpress
  
  sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
  
  sudo setsebool -P httpd_can_network_connect=1
  
## Step 4 — Install MySQL on your DB Server EC2

sudo yum update

sudo yum install mysql-server

Verify that the service is up and running by using sudo systemctl status mysqld, if it is not running, restart the service and enable it so it will be running even after reboot:

sudo systemctl restart mysqld

sudo systemctl enable mysqld

Step 5 — Configure DB to work with WordPress

sudo mysql

CREATE DATABASE wordpress;

CREATE USER `myuser`@`<Web-Server-Private-IP-Address>` IDENTIFIED BY 'mypass';

GRANT ALL ON wordpress.* TO 'myuser'@'<Web-Server-Private-IP-Address>';


FLUSH PRIVILEGES;
  
SHOW DATABASES;
  
exit
  
## Step 6 — Configure WordPress to connect to remote database.
  
Hint: Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, you shall allow access to the DB server ONLY from your Web Server’s IP address, so in the Inbound Rule configuration specify source as /32
  
<img width="1430" alt="Screenshot 2022-02-02 at 09 03 15" src="https://user-images.githubusercontent.com/97977311/152115824-b1ec199d-3266-4c1e-a51a-465ffdf68bcc.png">


Install MySQL client and test that you can connect from your Web Server to your DB server by using mysql-client
  
sudo yum install mysql
  
sudo mysql -u admin -p -h 172.31.81.93
  
Verify if you can successfully execute SHOW DATABASES; command and see a list of existing databases.

Change permissions and configuration so Apache could use WordPress:

Enable TCP port 80 in Inbound Rules configuration for your Web Server EC2 (enable from everywhere 0.0.0.0/0 or from your workstation’s IP)

Try to access from your browser the link to your WordPress http://<Web-Server-Public-IP-Address>/wordpress/
  
 ### Got Database connection error 
  <img width="1020" alt="error establishing DB connection" src="https://user-images.githubusercontent.com/97977311/152117220-127980e3-89c8-4735-8616-22aba36c9973.png">
  
  ### Issue was resolved by  opening mysqld binding config to the internet.
  
  <img width="833" alt="my cnf solved error connection" src="https://user-images.githubusercontent.com/97977311/152118403-63634e5f-aa88-4639-9722-fdc52c5e61c7.png">
  
## Wordpress Page
  <img width="1676" alt="my worpress" src="https://user-images.githubusercontent.com/97977311/152118745-6989a603-1cf9-439d-a280-e4bb71eec114.png">
  
  
  
