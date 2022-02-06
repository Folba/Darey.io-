## Step 1 – Install Jenkins server

### Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins" and Install JDK (since Jenkins is a Java-based application)

sudo apt update

sudo apt install default-jdk-headless

### Install Jenkins

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins

Confirm Jenkins is runnning

<img width="1055" alt="SG" src="https://user-images.githubusercontent.com/97977311/152698303-3559b7d1-39a6-4d17-9612-74a0d168b86b.png">

sudo systemctl status jenkins

<img width="1349" alt="jenkins_Status" src="https://user-images.githubusercontent.com/97977311/152697754-0bdf86f4-38d4-47b4-851c-ade9d360818d.png">

### Perform initial Jenkins setup.

### From your browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

### You will be prompted to provide a default admin password



