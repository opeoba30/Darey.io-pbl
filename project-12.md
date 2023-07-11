# Ansible Refactoring and Static Assignments (IMPORTS AND ROLES)

In the previous project, I implemented Configuration Managment solution on the Development Servers using Ansible i.e Automation of project 7 to 10

In this project, I will be broadening the performance of this architecture and introducing configurations for User Acceptance Testing (UAT) environment.

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/97306fe7-8804-407b-8570-0a8942c16c5c)

## STEP 1 - Jenkins Job Enhancement

1. Go to your Jenkins-Ansible server and create a new directory called ansible-config-artifact – we will store there all artifacts after each build.

   `sudo mkdir /home/ubuntu/ansible-config-artifact`
   
<img width="287" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/0db9ec51-3c29-4252-be59-66ae7607ea9a">

2. Change permissions to this directory, so Jenkins could save files there

`chmod -R 0777 /home/ubuntu/ansible-config-artifact`

3. Go to Jenkins web console -> Manage Jenkins -> Manage Plugins -> on Available tab search for Copy Artifact and install this plugin without restarting Jenkins

<img width="890" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/8ee377c9-a7c3-466f-853b-546b3a6ef6ca">

<img width="296" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/b33f70b0-ab86-46aa-a5f8-1e9072a70a06">

4. Create a new Freestyle project and name it save_artifacts.

5. This project will be triggered by completion of your existing ansible project. Configure it accordingly:
   
<img width="946" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/7a8e9715-8f3e-464d-a77b-cd32c96b0848">

<img width="440" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6579b905-f847-4edc-b032-d2ae7f29b82b">

Note: You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last 2 or 5 build results. You can also make this change to your ansible job.

I configured the number of build to 2. This is useful because whenever the jenkins pipeline runs, it creates a directory for the artifacts and it takes alot of space. By specifying the number of build, we can choose to keep only 2 of the latest builds and discard the rest

<img width="471" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/909f4483-2c2d-4a21-8d47-8d6552b5d027">

6. The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To achieve this, I created a Build step and chose Copy artifacts from other project, I specified ansible as a source project and /home/ubuntu/ansible-config-artifact as a target directory.

7. Test your set up by making some change in README.MD file inside your ansible-config-mgt repository (right inside main branch).

If both Jenkins jobs have completed one after another – you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your master branch.

Now your Jenkins pipeline is more neat and clean.

<img width="949" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/257137cc-fafc-4f20-8880-64fbc7b6ad9e">

<img width="248" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/79636f51-5dd9-4adb-9a8b-be513d5f4eed">

## Step 2 – Refactor Ansible code by importing other playbooks into site.yml

Before starting to refactor the codes, ensure that you have pulled down the latest code from master (main) branch, and created a new branch, name it refactor.

<img width="275" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/24455ead-feff-4e22-aea5-a5b0a6a744f4">

In Project 11 , I wrote all tasks in a single playbook common.yml, now it is pretty simple set of instructions for only 2 types of OS, but imagine you have many more tasks and you need to apply this playbook to other servers with different requirements. In this case, you will have to read through the whole playbook to check if all tasks written there are applicable and is there anything that you need to add for certain server/OS families. Very fast it will become a tedious exercise and your playbook will become messy with many commented parts. Your DevOps colleagues will not appreciate such organization of your codes and it will be difficult for them to use your playbook.

- In playbooks folder, create a new file and name it site.yml – This file will now be considered as an entry point into the entire infrastructure configuration.

- Create a new folder in root of the repository and name it static-assignments. The static-assignments folder is where all other children playbooks will be stored

- Move common.yml file into the newly created static-assignments folder.

- Inside site.yml file, import common.yml playbook.



















