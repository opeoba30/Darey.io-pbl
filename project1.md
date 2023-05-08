
## WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

A technology stack is a set of frameworks and tools used to develop a software product.They are acronymns for individual technologies used together for a specific technology product. some examples are… LAMP(Linux, Apache, MySQL, PHP or Python, or Perl), LEMP(Linux, Nginx, MySQL, PHP or Python, or Perl), MERN(MongoDB, ExpressJS, ReactJS, NodeJS), MEAN(MongoDB, ExpressJS, AngularJS, NodeJS)
### Preparing prerequisites
In order to complete this project i will need an AWS account and a virtual server with Ubuntu Server OS.
AWS is the biggest Cloud Service Provider and it offers a free tier account that am going to leverage for this project. Right now, AWS can provide me with a free virtual server called EC2 (Elastic Compute Cloud) for my needs

I signed in into my AWS account, selected my region (closest to me), and launch a new EC2 instance of t2.micro family with Ubuntu Server 20.04 LTS (HVM).
IMPORTANT – I saved private key (.pem file) for security purposes when launching my instance.

### Connecting to EC2 terminal (Using Windows Terminal)
I changed directory into the loacation where your PEM file is.   
  
i then Connected to the instance by running
  
I Installed Apache using Ubuntu’s package manager and verified its running

<img width="670" alt="apache2" src="https://user-images.githubusercontent.com/132816403/236711887-1abe04a5-68e5-47f4-99aa-4cb0d2602456.png">

I have TCP port 22 open by default on our EC2 machine to access it via SSH, so I need to add a rule to EC2 configuration to open inbound connection through port 80:

I access it locally in our Ubuntu shell, by running: curl http://localhost:80

I then opened a web browser of to access my Public-IP-Address
  
I installed a Database Management System (DBMS) MySQL to be able to store and manage data for my site in a relational database and run it. MySQL is a popular relational database management system used within PHP environments.

<img width="666" alt="MySQL1" src="https://user-images.githubusercontent.com/132816403/236711792-cc542333-f756-41e9-9274-3bceb4af0552.png">

## INSTALLING PHP 
I installed 3 php packages and run it to confirm the version

Then i created a virtual host for my site using apache

<img width="668" alt="php1" src="https://user-images.githubusercontent.com/132816403/236712339-3ae5902d-d800-4d0c-8566-fabbae05f25b.png">

I then enabled php on the website. image below

<img width="678" alt="proj 1" src="https://user-images.githubusercontent.com/132816403/236712615-4223028b-043e-4b80-bb49-6bb640981a24.png">

which brought me to the completion of my project.

<img width="956" alt="project1" src="https://user-images.githubusercontent.com/132816403/236712798-ee5abccf-86b4-4625-8e25-bed8f7d6f356.png">
