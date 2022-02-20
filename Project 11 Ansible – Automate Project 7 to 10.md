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

In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature. To do this clone the the repository to local repo

<img width="1517" alt="create branch " src="https://user-images.githubusercontent.com/97977311/154865418-133790ad-265c-4283-8f72-faa374602551.png">

#### Checkout the newly created feature branch to your local machine and start building your code and directory structure

#### Create a directory and name it playbooks – it will be used to store all your playbook files.


#### Create a directory and name it inventory – it will be used to keep your hosts organised.


#### Within the playbooks folder, create your first playbook, and name it common.yml


#### Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

<img width="1517" alt="mkdir and toch inventory and playbooks" src="https://user-images.githubusercontent.com/97977311/154865708-9da14400-23c5-4a33-91cb-04d62d698814.png">

#### Update your inventory/dev.yml file with this snippet of code:

[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

[db]
<Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
 
 #### Update your playbooks/common.yml file with following code:
 
 ---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
 
#### use git commands to add, commit and push your branch to GitHub.

git status

git add <selected files>

git commit -m "commit message" 
 
<img width="1517" alt="git add and git commit" src="https://user-images.githubusercontent.com/97977311/154866227-d1c6d3e6-4c7c-40e4-9047-05dd338113f3.png">
 
 #### Create a Pull request (PR)
 
 #### Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.
 
 
