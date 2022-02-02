## Step 1 – Prepare NFS Server
Spin up a new EC2 instance with RHEL Linux 8 Operating System.

Based on your LVM experience from Project 6, Configure LVM on the Server.

Instead of formating the disks as ext4 you will have to format them as xfs

Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs

Create mount points on /mnt directory for the logical volumes as follow:

Mount lv-apps on /mnt/apps – To be used by webservers

Mount lv-logs on /mnt/logs – To be used by webserver logs

Mount lv-opt on /mnt/opt to be utilized in the next project

sudo gdisk utility to partition the disk 

<img width="1231" alt="gdisk" src="https://user-images.githubusercontent.com/97977311/152122028-f62ac76e-9ba2-488f-bf34-b3e43670961b.png">

Physical volume with pvcreate utility

<img width="1231" alt="sudo pvs" src="https://user-images.githubusercontent.com/97977311/152122735-4e9d57fc-a97f-40b3-8fae-988f944b2ffc.png">

install lvm2 

<img width="1231" alt="install lvm2" src="https://user-images.githubusercontent.com/97977311/152123375-bfa99af3-c007-4d4b-916f-ae404f8c84b6.png">

Create Volume group with vgcreate utility and also create logical volume with lvcreate utility 

<img width="1161" alt="lvcreate" src="https://user-images.githubusercontent.com/97977311/152123850-4004b9e4-7768-4ba7-9cfa-3053f73740d0.png">
  
Format the logical volumes

<img width="1161" alt="mkfs" src="https://user-images.githubusercontent.com/97977311/152128707-4841ed15-bdb7-4f84-9cc2-ef7e43603f18.png">

### Create mount points on /mnt directory for the logical volumes as follow:

sudo mount /dev/webdata-vg/lv-aps  /mnt/apps

sudo mount /dev/webdata-vg/lv-logs /mnt/logs

### Install NFS server, configure it to start on reboot and make sure it is u and running

sudo yum -y update

sudo yum install nfs-utils -y

sudo systemctl start nfs-server.service

sudo systemctl enable nfs-server.service

sudo systemctl status nfs-server.service

### Make sure we set up permission that will allow our Web servers to read, write and execute files on NFS:

sudo chown -R nobody: /mnt/apps

sudo chown -R nobody: /mnt/logs

sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps

sudo chmod -R 777 /mnt/logs

sudo chmod -R 777 /mnt/opt

<img width="1161" alt="chown -R nobody" src="https://user-images.githubusercontent.com/97977311/152134557-4eeca07b-55b1-4729-a466-cd1ef4d65d2a.png">

<img width="665" alt="chmod -R 777" src="https://user-images.githubusercontent.com/97977311/152134581-69af2c69-8f38-45a0-bee1-e7e8476a32a9.png">

sudo systemctl restart nfs-server.service

### Configure access to NFS for clients within the same subnet (example of Subnet CIDR of webservers)
sudo vi /etc/exports

/mnt/apps Subnet-CIDR (rw,sync,no_all_squash,no_root_squash)
  
/mnt/logs Subnet-CIDR (rw,sync,no_all_squash,no_root_squash)
  
/mnt/opt Subnet-CIDR (rw,sync,no_all_squash,no_root_squash)

Esc + :wq!

sudo exportfs -arv

<img width="665" alt="exportfs" src="https://user-images.githubusercontent.com/97977311/152134755-43a391ec-6fa0-4af6-9071-1ac9b2b98058.png">

#### Check which port is used by NFS and open it using Security Groups (add new Inbound Rule)


Important note: In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049

rpcinfo -p | grep nfs

<img width="665" alt="rpcinfo -p grep nfs" src="https://user-images.githubusercontent.com/97977311/152136056-fb086d64-0bd5-4655-849c-9560069a2268.png">

<img width="974" alt="SG NFS" src="https://user-images.githubusercontent.com/97977311/152136574-3e93e921-9a0d-4062-9e82-86db8c1939dd.png">

## STEP 2 — CONFIGURE THE DATABASE SERVER

Install MySQL server

Create a database and name it tooling

Create a database user and name it webaccess

Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr

Grant all privileges on tooling.* to 'webaccess'@'172.31.16.0/20' identified by 'password';

<img width="931" alt="show databases" src="https://user-images.githubusercontent.com/97977311/152137457-e4d2e076-3345-439a-8892-8cd9132ff1cb.png">

## Step 3 — Prepare the Web Servers

Launch a new EC2 instance with RHEL 8 OS

#### Install NFS client

sudo yum install nfs-utils nfs4-acl-tools -y

#### Mount /var/www/ and target the NFS server’s export for apps

sudo mkdir /var/www

sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www  
  
<img width="1112" alt="df -h" src="https://user-images.githubusercontent.com/97977311/152139956-1f92b564-e923-4349-ae65-fdb116c73748.png">
  
  
Verify that NFS was mounted successfully by running df -h. Make sure that the changes will persist on Web Server after reboot:
  
<img width="1112" alt="df -h" src="https://user-images.githubusercontent.com/97977311/152139956-1f92b564-e923-4349-ae65-fdb116c73748.png">
  
sudo vi /etc/fstab
  
add following line

NFS-Server-Private-IP-Address:/mnt/apps /var/www nfs defaults 0 0
  
#### Install Remi’s repository, Apache and PHP
  
sudo yum install httpd -y

sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm

sudo dnf module reset php

sudo dnf module enable php:remi-7.4

sudo dnf install php php-opcache php-gd php-curl php-mysqlnd

sudo systemctl start php-fpm

sudo systemctl enable php-fpm

setsebool -P httpd_execmem 1
  
sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/logs /var/log/httpd  
  
sudo vi /etc/fstab
  
add following line

NFS-Server-Private-IP-Address:/mnt/logs /var/log/httpd nfs defaults 0 0
    
#### Repeat similar step for the other servers.
  
## Fork the tooling source code from Darey.io Github Account to your Github account. (Learn how to fork a repo here) 
  
Deploy the tooling website’s code to the Webserver. Ensure that the html folder from the repository is deployed to /var/www/html
  
<img width="1112" alt="git init and git clone" src="https://user-images.githubusercontent.com/97977311/152142815-a5055ae9-02ac-4ce3-9041-f946d434feed.png">
  
<img width="1112" alt="Screenshot 2022-01-30 at 15 47 27" src="https://user-images.githubusercontent.com/97977311/152143144-be62af4b-2035-49cc-a130-99d14dda738b.png">
  
TCP port 80 on the Web Server
  
disable SELinux sudo setenforce 0
  
sudo vi /etc/sysconfig/selinux and set SELINUX=disabled
  
<img width="1112" alt="Screenshot 2022-01-30 at 15 55 52" src="https://user-images.githubusercontent.com/97977311/152143503-f0359684-3717-459d-a270-899e013fdb5f.png">  
  
sudo setenforce 0  
  
Update the website’s configuration to connect to the database (in /var/www/html/functions.php file). 
  
Apply tooling-db.sql script to your database using this command mysql -h <databse-private-ip> -u <db-username> -p <db-pasword> < tooling-db.sql
                                                                                                                                               
<img width="1159" alt="update tooling DB" src="https://user-images.githubusercontent.com/97977311/152144152-94dd606c-317d-4ba8-b7ee-81a36dfb028a.png">   
  
Open the website in your browser http://<Web-Server-Public-IP-Address-or-Public-DNS-Name>/index.php and make sure you can login into the websute with myuser user.
  
 <img width="1159" alt="Login Page" src="https://user-images.githubusercontent.com/97977311/152146560-06ec439c-0838-4b66-bfd3-64019debfa77.png">

  
  <img width="1437" alt="Homepage" src="https://user-images.githubusercontent.com/97977311/152146553-5f640f05-2fa6-4f11-a81c-c5e19dc3fa07.png">
  
  
  
  
  
                                                                                                                                               
                                                                                                                                               











