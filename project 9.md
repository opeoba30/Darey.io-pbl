# TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. INTRODUCTION TO JENKINS

In this project we are going to start automating part of our routine tasks with a free and open source automation server – Jenkins. It is one of the most popular CI/CD tools, it was created by a former Sun Microsystems developer Kohsuke Kawaguchi and the project originally had a named "Hudson".

Acording to Circle CI, Continuous integration (CI) is a software development strategy that increases the speed of development while ensuring the quality of the code that teams deploy. Developers continually commit code in small increments (at least daily, or even several times a day), which is then automatically built and tested before it is merged with the shared repository.

In our project we are going to utilize Jenkins CI capabilities to make sure that every change made to the source code in GitHub https://github.com/<yourname>/tooling will automatically be updated to the Tooling Website.

## Task

Enhance the architecture prepared in Project 8 by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.

Here is how your updated architecture will look like upon competion of this project:

## INSTALL AND CONFIGURE JENKINS SERVER

Step 1 – Install Jenkins server.

    1. Create an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
    
  <img width="837" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/40505b5b-1ea0-4ea7-83a1-8f43729c1ee0">

    2. Install JDK (since Jenkins is a Java-based application)

 `sudo apt update` `sudo apt install default-jdk-headless -y`
    
    3. Install Jenkins
    
`curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null`
    
  `echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null`
  
  `sudo apt-get update` 
  `sudo apt-get install fontconfig openjdk-11-jre` 
  `sudo apt-get install jenkin`

**Make sure Jenkins is up and running**

<img width="582" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/d09c4ae4-102e-4232-84bb-98c217375412">

  4. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group

<img width="843" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/e35fa61f-cb1b-45dc-8be0-341db14b74b7">

5. Perform initial Jenkins setup.

   From your browser access http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080

   You will be prompted to provide a default admin password, Retrieve it from your server:

`sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

<img width="485" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/c02c74b8-f0d6-4d81-b8bf-f2b1d59aa85a">

Then you will be asked which plugings to install – choose suggested plugins.

<img width="415" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/7b0ed736-f81f-4fe8-9543-8b424a86758b">

Once plugins installation is done – create an admin user and you will get your Jenkins server address.

The installation is completed!

<img width="496" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/3d18f16e-c40d-4ec6-bc19-f88e9fd40e21">


## Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks

In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

1. Enable webhooks in your GitHub repository settings
  Go to the repository you want to use (source file) and select repository settings, then select webhooks and configure it like this below:

<img width="944" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/b606360b-1e5b-451f-bcb7-7aebb5300d1f">

<img width="947" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/bc4ff8a1-17cb-44a1-87ae-9d65d9c4cad6">

2.	Go to Jenkins web console, click "New Item" and create a "Freestyle project"

To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself

In configuration of your Jenkins freestyle project choose Git repository, provide there the link to your Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

<img width="702" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/aa25d456-eb0b-47f8-8bfb-ca0a29b7dc8c">

Save the configuration and let us try to run the build. For now we can only do it manually.
Click "Build Now" button, if you have configured everything correctly, the build will be successfull and you will see it under #1

You can open the build and check in "Console Output" if it has run successfully.

<img width="959" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/acea4312-0c00-41c2-8009-13bd60db2c4f">

If so – congratulations! You have just made your very first Jenkins build!

But this build does not produce anything and it runs only when we trigger it manually. Let us fix it.

3.	Click "Configure" your job/project and add these two configurations

Configure triggering the job from GitHub webhook:

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/0bd27fb2-6daf-41f3-9325-0ddf34a3b0d8)

Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".

<img width="684" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/df9966e3-fe23-44ba-8baf-942e7688620f">

<img width="382" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/0b274de0-e2b4-432a-a2c5-51710320a208">

when requested for file to archive, input ' ** ' to archive all,

<img width="451" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/61b342e0-f2f7-4e40-8c56-019ea7de9da7">

You have now configured an automated Jenkins job that receives files from GitHub by webhook trigger (this method is considered as ‘push’ because the changes are being ‘pushed’ and files transfer is initiated by GitHub). There are also other methods: trigger one job (downstreadm) from another (upstream), poll GitHub periodically and others.

By default, the artifacts are stored on Jenkins server locally

`ls /var/lib/jenkins/jobs/job-1/builds/4/archive`

<img width="454" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/fa9b9d21-d2f6-4725-aee6-15894fc54ece">

### Step 3 – Configure Jenkins to copy files to NFS server via SSH

Now we have our artifacts saved locally on Jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.

Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called "Publish Over SSH".

1. Install "Publish Over SSH" plugin.

  On "Available" tab search for "Publish Over SSH" plugin and install it
   
<img width="942" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/342cc134-7a80-4c27-a630-d7efea641358">

2. Configure the job/project to copy artifacts over to NFS server.

On main dashboard select "Manage Jenkins" and choose "System" menu item.

Scroll down to Publish over SSH plugin configuration section and configure it to be able to connect to your NFS server:

i. Provide a private key (content of .pem file that you use to connect to NFS server via SSH/Putty)

ii. Arbitrary name

iii. Hostname – can be private IP address of your NFS server

iv. Username – ec2-user (since NFS server is based on EC2 with RHEL 8)

v. Remote directory – /mnt/apps since our Web Servers use it as a mointing point to retrieve files from the NFS server

Test the configuration and make sure the connection returns Success. Remember, that TCP port 22 on NFS server must be open to receive SSH connections.

<img width="839" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/c2ea2350-9696-4f69-82d4-81c701fa5e3d">

<img width="713" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/326a92a6-ac6a-415a-82fb-a34617aa15ed">

Save the configuration, open your Jenkins job/project configuration page and add another one "Post-build Action"

Configure it to send all files probuced by the build into our previouslys define remote directory. In our case we want to copy all files and directories – so we use **.
If you want to apply some particular pattern to define which files to send – use this syntax.

Save this configuration and go ahead, change something in README.MD file in your GitHub Tooling repository.

Webhook will trigger a new job and in the "Console Output" of the job you will find something like this:

<img width="505" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/843487db-ce14-405f-835c-9d5dec6f3991">

To make sure that the files in /mnt/apps have been udated – connect via SSH/Putty to your NFS server and check README.MD file

`cat /mnt/apps/README.md`

<img width="351" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/e279b8ac-8097-458d-808d-a3611effb187">

If you see the changes you had previously made in your GitHub – the job works as expected.

Congratulations!

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6e6a2867-823e-4709-84df-abacfd39943d)






  
