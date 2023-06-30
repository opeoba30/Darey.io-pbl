# INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE
## Installing Ansible on Jenkins Server

1. Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible.
   
<img width="561" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/c2b8bed8-3bec-41c2-a38a-d3473284fbc9">
   
2. In your GitHub account create a new repository and name it ansible-config-mgt.

<img width="746" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/dcbbba37-cbae-4780-8c7d-c87e2db2afa5">

3. Install Ansible

`sudo apt update`
`sudo apt install ansible`

Check your Ansible version by running ansible --version


<img width="338" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6b0688c5-6cbe-4ed6-b7cb-78f69df90bcd">

4. Configure Jenkins build job to save your repository content every time you change it – this will solidify your Jenkins configuration skills acquired in Project 9.

  - Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.
  
  - Configure Webhook in GitHub and set webhook to trigger ansible build.
  
  - Configure a Post-build job to save all (**) files, like you did it in Project 9.

Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder

`ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/`


<img width="315" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/1d8f83b7-28e3-484b-b2fd-edd577bdfff2">

Now my setup looks like this:

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/b729fc9a-56ca-40e1-b3a6-bbdd718e2b4d)

Allocate Elastic_ip to jenkins-ansible server

<img width="750" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/06374ac9-77a4-4871-a74a-a26a01a16706">

## Step 2 – Prepare your development environment using Visual Studio Code

1. First part of ‘DevOps’ is ‘Dev’, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable – you need an Integrated development environment (IDE) or Source-code Editor. I will be using Visual Studio Code (VSC)

2. 2. After you have successfully installed VSC, configure it to connect to your newly created GitHub repository
  
      <img width="409" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/35ab7100-daba-491a-b8e8-d11282680697">
      
3. Clone down your ansible-config-mgt repo to your Jenkins-Ansible instance

`git clone <ansible-config-mgt repo link>`

<img width="323" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6f67f13b-eb04-4fe2-b43d-a589d1c8060f">


## BEGIN ANSIBLE DEVELOPMENT

1. In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.

   




