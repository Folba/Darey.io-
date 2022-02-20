 #### Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

 #### In your GitHub account create a new repository and name it ansible-config-mgt.

#### Install Ansible

sudo apt update

sudo apt install ansible

<img width="1371" alt="ansible version" src="https://user-images.githubusercontent.com/97977311/154841518-4d254ae8-4232-4ac3-ab9c-d10dc3d4e62a.png">

#### Configure Jenkins build job to save your repository content every time you change it – this will solidify your Jenkins configuration skills acquired in Project 9.

#### Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.

<img width="1311" alt="Jenkins to Ansible repo" src="https://user-images.githubusercontent.com/97977311/154843066-546a4d40-13d8-4cff-af0f-1c7a3ed51be9.png">

#### Configure Webhook in GitHub and set webhook to trigger ansible build.

<img width="1379" alt="webhook for Jenkins on github" src="https://user-images.githubusercontent.com/97977311/154841800-82817f33-5532-4416-8365-c0e18486ea79.png">

#### Configure a Post-build job to save all (**) files, like you did it in Project 9.

#### Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts)

<img width="1245" alt="build test check atifact" src="https://user-images.githubusercontent.com/97977311/154843263-df52d31d-95a7-4bbc-ae72-5800da43e8e5.png">
