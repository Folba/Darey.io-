## Register new domain and create hosted Zone on Route53 and create records accordingly

<img width="1198" alt="buy domain name" src="https://user-images.githubusercontent.com/97977311/153037439-cb9627f1-97fd-4683-97e6-dda9fc1aebf0.png">

<img width="1144" alt="point record to LB IP" src="https://user-images.githubusercontent.com/97977311/153038651-fb300c94-31ba-4064-b07c-3de17ca57e79.png">

<img width="1528" alt="hosted Zone " src="https://user-images.githubusercontent.com/97977311/153039837-92457575-1c49-4e11-b85a-192be32c62fe.png">

## Install ngix as LB on Ubuntu machine

sudo apt update

sudo apt instll nginx

## Configure load Balancer

sudo vi /etc/nginx/sites_avalable
this is wrong config, this gave error for nginx -t test when i added when (server1 server2 server3) 
<img width="1349" alt="wrong load_balancer Config" src="https://user-images.githubusercontent.com/97977311/153046827-a930cdea-43e4-4104-be3f-4cd4f27035de.png">
<img width="1343" alt="failed ngonx test" src="https://user-images.githubusercontent.com/97977311/153048681-3edb2e18-10e9-4555-94ed-394119226f63.png">

the server name should be server for all webservers like below
<img width="1002" alt="right config" src="https://user-images.githubusercontent.com/97977311/153047705-b4e7b134-bd7d-43aa-bcb9-2ccf2c25d86e.png">
<img width="1349" alt="nginx -t" src="https://user-images.githubusercontent.com/97977311/153048826-f1697998-38c3-4565-89f5-05fbe90a6b7b.png">

From site-enabled driectory link to sites-available/load_balancer.conf and also delete the default config in sites-enabled

rm -f /etc/nginx/sites-enabled/default
<img width="1384" alt="remove default config enabled site" src="https://user-images.githubusercontent.com/97977311/153051449-2abbc3d4-1037-42d0-b9a1-b505ab9f0732.png">
<img width="1190" alt="link" src="https://user-images.githubusercontent.com/97977311/153050874-d99b9f3a-a49e-4267-98db-80b881c52eac.png">

<img width="1349" alt="ll for sites enabled" src="https://user-images.githubusercontent.com/97977311/153051794-26e2b144-5e83-4405-ad63-726c39a7a060.png">

Restart Nginx and confirm status
<img width="1517" alt="status" src="https://user-images.githubusercontent.com/97977311/153052901-5447feb1-677a-4d10-a66d-973e840f255c.png">

# Confirm the the the domain name url on browser
www.folbatooling.click
<img width="1325" alt="unsecured website" src="https://user-images.githubusercontent.com/97977311/153054166-ac27d92c-b11c-4d9c-b05e-7e432140a9c3.png">

Install certificate using cerbot to maek the domain secured

sudo systemctl status snapd

<img width="1668" alt="Check Snap is working" src="https://user-images.githubusercontent.com/97977311/153055224-10199879-7c53-472d-ab8f-0c1c7abcf5d4.png">


sudo snap install --classic certbot


sudo ln -s /snap/bin/certbot /usr/bin/certbot


sudo certbot --nginx


Test secured access to your Web Solution by trying to reach https://<your-domain-name.com>

<img width="1430" alt="secured" src="https://user-images.githubusercontent.com/97977311/153055100-a2b1c884-c784-4146-b2c9-9ffe9b636bb6.png">

Create a cronjob by editing crontab with the command below.

crontab -e

* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1

<img width="1168" alt="cronjob" src="https://user-images.githubusercontent.com/97977311/153058984-a2659de8-d134-45f0-96b7-1f257063d18a.png">
