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
