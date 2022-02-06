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
    
### Step 2 Go to Jenkins web console, click "New Item" and create a "Freestyle project"    

<img width="1074" alt="Create Project Freestyle" src="https://user-images.githubusercontent.com/97977311/152699173-dbeb25a9-c885-47da-a52d-e4da21ffb5c4.png">
    
### To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself

### In configuration of your Jenkins freestyle project choose Git repository, provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.
    
<img width="1281" alt="Add Git Source " src="https://user-images.githubusercontent.com/97977311/152699380-d577180e-67b6-4824-a288-66f0d4a86651.png">
    
<img width="1319" alt="add credentials" src="https://user-images.githubusercontent.com/97977311/152699394-07c809ec-5334-4f45-9411-84a38a60b653.png">
    
    
### Save the configuration and let us try to run the build. For now we can only do it manually.
    
### Click "Build Now" button, if you have configured everything correctly, the build will be successfull and you will see it under #1
    
### Click "Configure" your job/project Configure triggering the job from GitHub webhook: and Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".

### Now, go ahead and make some change in any file in your GitHub repository (e.g. README.MD file) and push the changes to the master branch.

### You will see that a new build has been launched automatically (by webhook) and you can see its results – artifacts, saved on Jenkins server.
    
<img width="1319" alt="console output 3" src="https://user-images.githubusercontent.com/97977311/152700556-114e6746-4c59-48d6-b187-3cfda5c7ea2e.png">

ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/
    
<img width="1319" alt="lastest read me build" src="https://user-images.githubusercontent.com/97977311/152700616-77cf7674-48d6-4c40-a752-520cc4f5cf2d.png">

