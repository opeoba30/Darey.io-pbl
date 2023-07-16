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

  <img width="352" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/2a0309a6-eebd-4d7f-8892-557477fe9b71">

- Inside site.yml file, import common.yml playbook.

<img width="490" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/48c9df08-b84e-4e61-bc96-f773c9fe6844">

- Run ansible-playbook command against the dev environment

- create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.

      ---
      - name: update web, nfs and db servers
        hosts: webservers, nfs, db
        remote_user: ec2-user
        become: yes
        become_user: root
        tasks:
        - name: delete wireshark
          yum:
            name: wireshark
            state: removed

      - name: update LB server
        hosts: lb
        remote_user: ubuntu
        become: yes
        become_user: root
        tasks:
        - name: delete wireshark
          apt:
            name: wireshark-qt
            state: absent
            autoremove: yes
            purge: yes
            autoclean: yes
  
- We update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers

      cd /home/ubuntu/ansible-config-mgt/
  
      ansible-playbook -i inventory/dev.yml playbooks/site.yml

<img width="406" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6f32e269-98f2-46c9-ad2e-36d870095903">

- Make sure that wireshark is deleted on all the servers by running wireshark --version

<img width="193" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/a62f0dc9-dd98-441e-be9d-dd7a95b02f8f">


## CONFIGURE UAT WEBSERVERS WITH A ROLE ‘WEBSERVER’

- Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers, so give them names accordingly – Web1-UAT and Web2-UAT.

  <img width="383" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/70a8bad4-e572-4ecf-9842-723fc6592aa5">

- Create a role using an Ansible utility called ansible-galaxy inside ansible-config-mgt/roles directory (you need to create roles directory upfront)

        mkdir roles
        cd roles
        ansible-galaxy init webserver

  <img width="308" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/4c514449-e52d-42ce-b9b5-1b37e27a2c58">

- removing unnecessary directories and files, the roles structure should look like this

  <img width="230" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6f06133e-a8d1-472a-96a5-405164c4e21a">

- Update your inventory ansible-config-mgt/inventory/uat.yml file with IP addresses of your 2 UAT Web servers

      [uat-webservers]
      <Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' 

      <Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user' 

- In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory roles_path    = /home/ubuntu/ansible-config-mgt/roles, so Ansible could know where to find configured roles.

  <img width="318" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/4d75ed14-9e06-4117-be73-831dbd84a234">

- It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:

      Install and configure Apache (httpd service)
      Clone Tooling website from GitHub https://github.com/<your-name>/tooling.git.
      Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
      Make sure httpd service is started
      Your main.yml may consist of following tasks:


Add the following to the main.yml of the webserver role

      ---
      - name: install apache
        become: true
        ansible.builtin.yum:
          name: "httpd"
          state: present

      - name: install git
        become: true
        ansible.builtin.yum:
          name: "git"
          state: present

      - name: clone a repo
        become: true
        ansible.builtin.git:
          repo: https://github.com/<your-name>/tooling.git
          dest: /var/www/html
          force: yes

      - name: copy html content to one level up
        become: true
        command: cp -r /var/www/html/html/ /var/www/

      - name: Start service httpd, if not started
        become: true
        ansible.builtin.service:
          name: httpd
          state: started

      - name: recursively remove /var/www/html/html/ directory
        become: true
        ansible.builtin.file:
          path: /var/www/html/html
          state: absent


## REFERENCE WEBSERVER ROLE

### Step 4 – Reference ‘Webserver’ role

- In the static-assignments folder, we create a new assignment for uat-webservers `uat-webservers.yml`.  This is where you will reference the role.

        ---
      - hosts: uat-webservers
        roles:
           - webserver
  
<img width="419" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/f89a42cb-6505-4073-b9e9-88b218698bb5">

Since the entry point to our ansible configuration is the `site.yml` file. Therefore, we need to refer our `uat-webservers.yml` role inside site.yml.

So, we should have this in `site.yml`

      ---
      - hosts: all
      - import_playbook: ../static-assignments/common.yml

      - hosts: uat-webservers
      - import_playbook: ../static-assignments/uat-webservers.yml

<img width="472" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/2b6a10d1-9d05-4888-af1c-b92b142480f7">

- Commit your changes, create a Pull Request and merge them to master branch, make sure webhook triggered two consequent Jenkins jobs, they ran successfully and copied all the files to your Jenkins-Ansible server into /home/ubuntu/ansible-config-mgt/ directory.

<img width="290" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/fd816e0c-def1-444b-b82c-b42ebfd5087d">

  <img width="476" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/36dbf53f-ed4a-418f-a0ed-09898ebdf5c3">

<img width="947" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/16fe2675-ca24-4f15-b05f-2da3e1745092">

Now run the playbook against your uat inventory and see what happens:

`ansible-playbook -i /home/ubuntu/ansible-config-mgt/inventory/uat.yml /home/ubuntu/ansible-config-mgt/playbooks/site.yml`

<img width="428" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/ffe6c582-ce59-44d4-a952-add572b53af9">

<img width="904" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/07964274-ae8f-4618-81f8-c883ca371779">

Your Ansible architecture now looks like this:

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/4d3a4f62-2720-4455-8f84-b9970603b32a)

Congratulations!
We have learned how to deploy and configure UAT Web Servers using Ansible imports and roles!

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/05c08c02-c3cb-44c1-9827-12fa0dd46f15)





