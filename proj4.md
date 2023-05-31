# MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

Firstly, I launched my EC2 instance on aws.

<img width="769" alt="EC2" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/a073ce46-0f09-48da-b29e-7ba21cc6a6b0">

Then open my terminal, cd into the directory i have my key and connect to my server.

## Step 1: Install NodeJs

Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used here to set up the Express routes and AngularJS controllers.

`sudo apt update`

`sudo apt upgrade -y`

### Add certificates

`sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates`

`curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

`sudo apt install -y nodejs`

## step 2: Installing MongoDB

MongoDB stores data in flexible, JSON-like documents. For our example application, I am adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

note: To avoid intallation errors in MongoDB I lauched Ubuntu 20.04 version instead of the latest version 22.04 on aws.

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`

`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

### Installing MongoDB

`sudo apt install -y mongodb`

Started The server by running   `sudo service mongodb start`

I verified that the service is up and running   `sudo systemctl status mongodb`

<img width="924" alt="mongoDB" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/93b1c8cc-b5cd-4370-9265-3de3bbe9a6f5">

# Installing npm

`sudo apt-get install aptitude`

`sudo aptitude install npm`

### Install body-parser package

I need ‘body-parser’ package to help me process JSON files passed in requests to the server.

`sudo npm install body-parser`

<img width="441" alt="bodyparser" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/9a21b9b1-764b-4000-9398-73a6cc28d905">

Created a folder named ‘Books’  `mkdir Books && cd Books`

In the Books directory, Initialized npm project  `npm init`

Add a file to it named server.js  `vi server.js`

i inseted the code in the image below

<img width="504" alt="1code" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/dbfef230-b8cd-4131-a04d-9cfdb633f9a8">

# SERVER
## INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER
Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. I will use Express in to pass book information to and from our MongoDB database, and also use Mongoose package which provides a straight-forward, schema-based solution to model your application data. I will use Mongoose to establish a schema for the database to store data of my book register.

`sudo npm install express mongoose`

<img width="436" alt="Mongoose" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/3f355f89-9270-407c-870d-426094da5614">

In ‘Books’ folder, I created a folder named apps and cd into it  `mkdir apps && cd apps`

I Created a file named routes.js  `vi routes.js` and wrote this code below into it.

<img width="286" alt="book_app_code" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/0fb56b62-6c63-4290-b174-3df2788d7b3b">

In the ‘apps’ folder, I created a folder named models and cd into it `mkdir models && cd models`

I created another file named book.js `vi book.js`

<img width="359" alt="3_book_code" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/a89d1486-6ea4-461b-b738-977a5cad2e8e">

## Step 4 – Access the routes with AngularJS

AngularJS provides a web framework for creating dynamic views in your web applications. In this project, I use AngularJS to connect our web page with Express and perform actions on book register.

Changed the directory back to ‘Books’   `cd ../..`

Created a folder named public   `mkdir public && cd public`

Added a file named script.js   `vi script.js`

<img width="242" alt="controller_config" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/59c1e4ae-4654-47e7-8cb9-3d37a0210329">

In public folder, create a file named index.html;  `vi index.html`

<img width="392" alt="index html" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/92a0693b-1ec1-4bf1-9711-6b49af222ef2">

Changed the directory back up to Books  `cd ..`

Started the server by running this command:   `node server.js`

The server is now up and running, I can connect it via port 3300.

It shall return an HTML page, it is hardly readable in the CLI, but I tried and access it from the Internet.

For this, i needed to open TCP port 3300 in my AWS Web Console for my EC2 Instance.

<img width="773" alt="inbound_rule" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/c3c67cfb-071e-40e0-9fa7-726d1a21ae06">

Now I can access my Book Register web application from the Internet with a browser using Public IP address or Public DNS name.

Quick reminder how to get your server’s Public IP and public DNS name:

You can find it in your AWS web console in EC2 details

Run `curl -s http://169.254.169.254/latest/meta-data/public-ipv4` for Public IP address

`curl -s http://169.254.169.254/latest/meta-data/public-hostname` for Public DNS name.

This is how my Web Book Register Application looks like in browser:

<img width="355" alt="proj4_comp" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/f3bdda18-c807-4a08-b3f0-86fd6cf796a7">
