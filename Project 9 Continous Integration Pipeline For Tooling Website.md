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

### Retrieve it from your server:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    
### Once plugins installation is done – create an admin user and you will get your Jenkins server address.

### The installation is completed!    
    
<img width="1009" alt="Jenkins_Is_Ready" src="https://user-images.githubusercontent.com/97977311/152698541-356eff1b-a2e3-46d6-8c3f-fea48446856d.png">    
    
### Go to Jenkins web console, click "New Item" and create a "Freestyle project"    

<img width="1074" alt="Create Project Freestyle" src="https://user-images.githubusercontent.com/97977311/152699173-dbeb25a9-c885-47da-a52d-e4da21ffb5c4.png">
    

