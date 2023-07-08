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
   
   Give your branches descriptive and comprehensive names, for example, if you use Jira or Trello as a project management tool – include ticket number (e.g. PRJ-145)

2. Checkout the newly created feature branch to your local machine and start building your code and directory structure

   <img width="293" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/34204199-1f46-445d-bd80-30f31e5fd650">

3. Create a directory and name it playbooks – it will be used to store all your playbook files.

4. Create a directory and name it inventory – it will be used to keep your hosts organised.

<img width="235" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/92d64228-cda3-4087-9d86-4ffda822b351">

5. Within the playbooks folder, create your first playbook, and name it common.yml

    <img width="220" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6ba4f83b-dc6c-4e8c-a83a-bdede0df57fb">
    
6. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

## Step 4 – Set up an Ansible Inventory

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs.

Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent. Now you need to import your key into ssh-agent:

`eval `ssh-agent -s``
`ssh-add <path-to-private-key>`

<img width="244" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/189d7227-1235-48d0-8edf-1af3f218935b">

Confirm the key has been added with the command below, you should see the name of your key

`ssh-add -l`

Now, ssh into your Jenkins-Ansible server using ssh-agent

`ssh -A ec2-user@public-ip` and for ubuntu user `ssh -A ubuntu@public-ip`

<img width="312" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/f48b816c-7c48-4673-bfd6-69d27d2a3556">



Also notice, that your Load Balancer user is ubuntu and user for RHEL-based servers is ec2-user.

Update your inventory/dev.yml file with this snippet of code:

<img width="423" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/a05a467c-d1c3-42c7-8262-bbaaf7af4cce">

<img width="221" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/e76c583a-1cbe-4bff-a8f6-110531dcb5b6">


## CREATE A COMMON PLAYBOOK

### Step 5 – Create a Common Playbook

It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.

In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

Update your playbooks/common.yml file with following code:

<img width="434" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/302650b1-2b52-4646-b7bc-62d253c74175">

but there will be a failure in one of the server "db" when you run your playbook because the db uses (apt) and not (yum) 

Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

run the ansible playbook command `ansible-playbook -i inventory/dev.yml playbooks/common.yml`

Feel free to update this playbook with following tasks:

Create a directory and a file inside it
Change timezone on all servers
Run some shell script
…

   
## Step 6 – Update GIT with the latest code

Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.

Commit your code into GitHub:

1. use git commands to add, commit and push your branch to GitHub.

`git status`

`git add <selected files>`

<img width="316" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/19f3fbb8-ab70-43dd-bc30-073e5878277f">

`git commit -m "commit message"`

<img width="319" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/7d108fa8-1b5a-41fe-89c2-a07926f9e5a9">


2. Create a Pull request (PR)

<img width="310" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/5bfde762-b504-4ca1-b05f-fbb9977e8333">

<img width="839" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/03be0199-28e4-40b9-968a-4229c850e31c">


3. Wear a hat of another developer for a second, and act as a reviewer.

4. If the reviewer is happy with your new feature development, merge the code to the master branch.

<img width="263" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/b036f6e4-80b1-4b75-8ee7-8a71130d8c9d">

5. Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.

<img width="320" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/dcd831f5-dbd9-4a94-b520-deb19f3bcbbb">

Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.

<img width="677" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/eaa234c1-463f-45c6-a737-b3c1549e9d96">

it triggers another build on my Jenkins, see below:

<img width="497" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/1f00210f-a14b-41ed-9e4a-d3544bc7578f">

Here below in Jenkins-Ansible server

<img width="266" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/fc460726-db87-402c-8014-5127f3948219">

## RUN FIRST ANSIBLE TEST

Now, it is time to execute ansible-playbook command and verify if your playbook actually works:

   `cd ansible-config-mgt`
   `ansible-playbook -i inventory/dev.yml playbooks/common.yml`
   
   You can go to each of the servers and check if wireshark has been installed by running `which wireshark` or `wireshark --version`
   
   <img width="314" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/c62cfcec-d00a-48d8-a597-525644582d4a">

   Your updated with Ansible architecture now looks like this:

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/847eb1ca-221b-4c84-a95e-1e42b837a4dc)

## Optional step – Repeat once again

Update your ansible playbook with some new Ansible tasks and go through the full checkout -> change codes -> commit -> PR -> merge -> build -> ansible-playbook cycle again to see how easily you can manage a servers fleet of any size with just one command!

## Congratulations

You have just automated your routine tasks by implementing your first Ansible project! There is more exciting projects ahead, so lets keep it moving!

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/52ba7ba8-93a5-4fbc-b1fe-18bc425e65bb)




   






