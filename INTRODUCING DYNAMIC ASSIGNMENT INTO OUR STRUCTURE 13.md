# INTRODUCING DYNAMIC ASSIGNMENT INTO OUR STRUCTURE

Introducing Dynamic Assignment Into Our structure

In this project, we introduced the concept of the use of Dynamic assignments. The major difference between static and dynamic assignments is in the use of `import and include ststements`.

Include does similar thing as Imports but differs because it is dynamic. Dynamic in the sense that ansible is able pick changes in playbooks added to a master playbook in real time. In import ansible preprocesses everything runs the playbook with the data it has. Changes made while executing the playbook is ignored.

Created a role-feature branch to implement the dynamic assignments of ansible

<img width="319" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/de16f50e-dde9-4c34-9cef-b26970b1f49c">

Create a new folder, name it `dynamic-assignments`. Then inside this folder, create a new file and name it `env-vars.yml`. We will instruct `site.yml` to `include` this playbook later. For now, let us keep building up the structure.

Your GitHub shall have following structure by now.

      ├── dynamic-assignments
      │   └── env-vars.yml
      ├── inventory
      │   └── dev
          └── stage
          └── uat
          └── prod
      └── playbooks
          └── site.yml
      └── roles (optional folder)
          └──...(optional subfolders & files)
      └── static-assignments
          └── common.yml

Since we will be using the same Ansible to configure multiple environments, and each of these environments will have certain unique attributes, such as servername, ip-address etc., we will need a way to set values to variables per specific environment.

For this reason, we will now create a folder to keep each environment’s variables file. Therefore, create a new folder `env-vars`, then for each environment, create new YAML files which we will use to set variables.

Your layout should now look like this.

    ├── dynamic-assignments
    │   └── env-vars.yml
    ├── env-vars
        └── dev.yml
        └── stage.yml
        └── uat.yml
        └── prod.yml
    ├── inventory
        └── dev
        └── stage
        └── uat
        └── prod
    ├── playbooks
        └── site.yml
    └── static-assignments
        └── common.yml
        └── webservers.yml

Now paste the instruction below into the env-vars.yml file.

    ---
    - name: collate variables from env specific file, if it exists
      hosts: all
      tasks:
        - name: looping through list of available files
          include_vars: "{{ item }}"
          with_first_found:
            - files:
                - dev.yml
                - stage.yml
                - prod.yml
                - uat.yml
              paths:
                - "{{ playbook_dir }}/../env-vars"
          tags:
            - always
Notice 3 things to notice here:

1. We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are:
- include_role
- include_tasks
- include_vars
  
In the same version, variants of import were also introduces, such as:

- import_role
- import_tasks
  
2. We made use of a special variables { playbook_dir } and { inventory_file }. { playbook_dir } will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem. { inventory_file } on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder.
3. We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment specific env file does not exist.

## UPDATE SITE.YML WITH DYNAMIC ASSIGNMENTS

Update site.yml file to make use of the dynamic assignment. (At this point, we cannot test it yet. We are just setting the stage for what is yet to come. So hang on to your hats)

`site.yml` should now look like this.

      ---
      - hosts: all
      - name: Include dynamic variables 
        tasks:
        import_playbook: ../static-assignments/common.yml 
        include: ../dynamic-assignments/env-vars.yml
        tags:
          - always
      
      -  hosts: webservers
      - name: Webserver assignment
        import_playbook: ../static-assignments/webservers.yml

<img width="571" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/4adca339-b0b4-4c1b-9f2c-812207ff502e">


## Community Roles

Now it is time to create a role for MySQL database – it should install the MySQL package, create a database and configure users. But why should we re-invent the wheel? There are tons of roles that have already been developed by other open source engineers out there. These roles are actually production ready, and dynamic to accomodate most of Linux flavours. With Ansible Galaxy again, we can simply download a ready to use ansible role, and keep going.

### Download Mysql Ansible Role

We will be using a `MySQL role developed by geerlingguy`.

### Hint: 
To preserve your your GitHub in actual state after you install a new role – make a commit and push to master your ‘ansible-config-mgt’ directory. Of course you must have git installed and configured on Jenkins-Ansible server and, for more convenient work with codes, you can configure Visual Studio Code to work with this directory. In this case, you will no longer need webhook and Jenkins jobs to update your codes on Jenkins-Ansible server, so you can disable it – we will be using Jenkins later for a better purpose.



On Jenkins-Ansible server make sure that `git` is installed with `git --version`, then go to ‘ansible-config-mgt’ directory and run

            git init
            git pull https://github.com/<your-name>/ansible-config-mgt.git
            git remote add origin https://github.com/<your-name>/ansible-config-mgt.git
            git branch roles-feature
            git switch roles-feature

Inside `roles` directory create your new MySQL role with `ansible-galaxy install geerlingguy.mysql` and rename the folder to `mysql`

            mv geerlingguy.mysql/ mysql

<img width="345" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/b7036904-acb1-4ff4-978b-a0317305b340">


Read `README.md` file, and edit roles configuration to use correct credentials for MySQL required for the `tooling` website.

Now it is time to upload the changes into your GitHub:













          
