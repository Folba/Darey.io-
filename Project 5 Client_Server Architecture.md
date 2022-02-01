# Implement a Client Server Architecture using MySQL Database Management System (DBMS).
#Create and configure two Linux-based virtual servers (EC2 instances in AWS).

#Server A name - `mysql server`

#Server B name - `mysql client


Open port for all traffic on DB and Client Servers security group respectively.

<img width="1369" alt="Screenshot 2022-02-01 at 14 01 50" src="https://user-images.githubusercontent.com/97977311/151973206-dc51de60-b9ba-4767-a4ba-a6c2b3f0e5bb.png">

<img width="977" alt="Screenshot 2022-02-01 at 12 37 24" src="https://user-images.githubusercontent.com/97977311/151961918-99c12864-ed36-4c7b-8eb9-b96bd7526310.png">


#update server software package.

sudo apt update

<img width="1043" alt="Screenshot 2022-01-12 at 12 54 08" src="https://user-images.githubusercontent.com/97977311/151946027-ce96aee0-d566-4cda-97b8-a6a74f26493b.png">

#Install mysql on both client and DB servers.

sudo apt install mysql-server

<img width="1661" alt="Screenshot 2022-01-12 at 13 11 05" src="https://user-images.githubusercontent.com/97977311/151952735-ea992f51-f956-4acf-ab65-870ca72bc733.png">

sudo mysql_secure_installation

<img width="1069" alt="Screenshot 2022-01-12 at 13 13 07" src="https://user-images.githubusercontent.com/97977311/151946134-a771c3aa-1054-42c8-9407-0c66d5b95de5.png">
sudo mysql
<img width="786" alt="Screenshot 2022-01-12 at 13 20 23" src="https://user-images.githubusercontent.com/97977311/151946171-f3c0fb7b-0362-40e6-a98d-edb84b680610.png">

#change binding address to 0.0.0.0 to allow connection to the database

sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf


<img width="1013" alt="Screenshot 2022-02-01 at 12 31 28" src="https://user-images.githubusercontent.com/97977311/151961551-42cfa76d-34bd-4c4e-911e-8015d9ef82b5.png">

Create DB

create user and grant persimision to to acecss from remote

<img width="977" alt="Screenshot 2022-02-01 at 13 16 12" src="https://user-images.githubusercontent.com/97977311/151969796-f939cc25-0ea0-4c62-8179-d146a3b524b7.png">

<img width="977" alt="Screenshot 2022-02-01 at 13 21 18" src="https://user-images.githubusercontent.com/97977311/151970245-551d5406-facc-4189-9c49-4befd38b7b5c.png">

test user access from remote 

<img width="988" alt="Screenshot 2022-02-01 at 14 10 12" src="https://user-images.githubusercontent.com/97977311/151974453-994861c0-e133-4a73-aa1e-853d3c2f4fb9.png">
