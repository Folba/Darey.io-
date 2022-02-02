# Configure Apache As A Load Balancer
Create an Ubuntu Server 20.04 EC2 

Install Apache Load Balancer on EC2 LB server server and configure it to point traffic coming to LB to the Web Servers:

## Install apache2
sudo apt update

sudo apt install apache2 -y

sudo apt-get install libxml2-dev

## Enable following modules:
sudo a2enmod rewrite

sudo a2enmod proxy

sudo a2enmod proxy_balancer

sudo a2enmod proxy_http

sudo a2enmod headers

sudo a2enmod lbmethod_bytraffic

#Restart apache2 service

sudo systemctl restart apache2

## Confirm if Apche is running

sudo systemctl status apache2

## Configure load balancing

sudo vi /etc/apache2/sites-available/000-default.conf

## Add this configuration into this section <VirtualHost *:80> 



</VirtualHost>

<Proxy "balancer://mycluster">

               BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
               BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/
        
        
<img width="1271" alt="Screenshot 2022-01-31 at 22 38 30" src="https://user-images.githubusercontent.com/97977311/152150799-8d519600-69b9-4d58-b5b3-bf0e2b458ff9.png">

## Restart apache server

sudo systemctl restart apache2

sudo systemctl status apache2

<img width="906" alt="Screenshot 2022-01-31 at 22 33 39" src="https://user-images.githubusercontent.com/97977311/152151208-2c94e382-3178-4bc4-908d-429cec01b192.png">


