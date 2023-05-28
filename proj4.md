# MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

Having launch my instance on aws, i then ran the following commands on my Terminal 

`sudo apt update`

`sudo apt upgrade -y`

`sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates`

`curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

`sudo apt install -y nodejs`

# Installing MongoDB

MongoDB stores data in flexible, JSON-like documents. For our example application, I am adding book records to MongoDB that contain book name, isbn number, author, and number of pages.

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`

`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

### Installing MongoDB

`sudo apt install -y mongodb`

Started The server by running   `sudo service mongodb start`

I verified that the service is up and running   `sudo systemctl status mongodb`

# Installing npm

`sudo apt-get install aptitude`

`sudo aptitude install npm`

### Install body-parser package

I need ‘body-parser’ package to help me process JSON files passed in requests to the server.

`sudo npm install body-parser`

Created a folder named ‘Books’  `mkdir Books && cd Books`

In the Books directory, Initialized npm project  `npm init`

Add a file to it named server.js  `vi server.js`

i inseted the code in the image below

<img width="504" alt="1code" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/dbfef230-b8cd-4131-a04d-9cfdb633f9a8">

# INSTALL EXPRESS AND SET UP ROUTES TO THE SERVER
Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. I will use Express in to pass book information to and from our MongoDB database, and also use Mongoose package which provides a straight-forward, schema-based solution to model your application data. I will use Mongoose to establish a schema for the database to store data of my book register.

`sudo npm install express mongoose`

In ‘Books’ folder, I created a folder named apps and cd into it  `mkdir apps && cd apps`

Create a file named routes.js  `vi routes.js`

