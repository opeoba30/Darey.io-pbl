# DOCUMENTATION FOR PROJECT 3

## SIMPLE TO-DO APPLICATION ON MERN WEB STACK
I lauched my EC2 instance on AWS as usual
BACKEND CONFIGURATION

Update ubuntu

`sudo apt update`

Upgrade ubuntu

`sudo apt upgrade`

I try to get the location of Node.js software from Ubuntu repositories.

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

<img width="868" alt="nodeJS" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/93e687a6-3645-4a00-9b8d-7a8d2cf5aa04">

Install Node.js on the server with the command below

`sudo apt-get install -y nodejs`

<img width="571" alt="nodeJS 2" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/c4ee34bb-7b5d-4ae6-9f34-94fb1df66f74">

so i verified the nodeJS installation  with 

`node -v` and  `npm -v`

Application Code Setup

I created a new directory for my To-Do project:

`mkdir Todo`

I ran the command ls to verify that the Todo directory is created with ls command

after i have confirmed the directory has been created, i now changed my current directory to the newly created one:

`cd Todo`

Next, i run a command `npm init` to initialise my project, so that a new file named package.json will be created.

<img width="602" alt="npm init" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/33faf29e-8e6b-43b6-8f9a-1e29bae19be2">

i run `ls` command to confirm that I have package.json file created.

Next, I Installed ExpressJs and create the Routes directory.

### INSTALL EXPRESSJS

To use express, I installed it using npm:   `npm install express`

Now I create a file index.js with the command   `touch index.js`

I run `ls` to confirm that my index.js file is successfully created

<img width="511" alt="index js" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/08ff03f3-70fe-42d7-adbe-a21dcac8bd08">

I Installed  the dotenv module `npm install dotenv`

<img width="479" alt="dotenv" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6531c0de-e276-4ec6-baf0-a72a6b43aebb">

I opened the index.js file with the command `vim index.js`

I opened your terminal in the same directory as my index.js file and type  `node index.js`

<img width="322" alt="5000 server" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/f4204771-faa2-4b93-a16e-5e96f33acb87">

Now i opened the port in EC2 Security Groups and set inbound rules for port 5000

<img width="734" alt="inbound rule" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/5d6017e1-86e9-4cff-b488-2eb804a4eea5">

I opened up your browser and try to access my server’s Public IP address followed by port 5000:

`http://<PublicIP-or-PublicDNS>:5000`

<img width="301" alt="express" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/203ec326-632f-492f-bd99-3aed8046fc1a">

### Routes

There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and I use different standard HTTP request methods: POST, GET, DELETE.

For each task, I created routes that will define various endpoints that the To-do app will depend on. So I created a folder routes

`mkdir routes`

and i changed to the routes directory `cd routes`

Now, I createD a file api.js with the command    `touch api.js`

I Opened the file with the command   `vim api.js`

I inserted the below code in the file.

`const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;`

Next i created Models directory.  
## MODELS

A model is at the heart of JavaScript based applications, and it is what makes it interactive.

I also use models to define the database schema . This is important so that I will be able to define the fields stored in each Mongodb document.

In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties

To create a Schema and a model, I installed mongoose which is a Node.js package that makes working with mongodb easier.

I Changed directory back Todo folder with `cd ..` and install Mongoose  `npm install mongoose`

Create a new folder models :  `mkdir models`

I changed directory into the newly created ‘models’ folder with  `cd models`

Inside the models folder,I created a file and name it todo.js  `touch todo.js`

<img width="503" alt="mongoose" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/0e42c67b-9eea-48af-8d70-f51063596487">

I opened the file created with `vim todo.js` then paste the code below in the file:

<img width="403" alt="file 3" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/09e5d328-ffcf-4606-ac61-ac806899edd4">

I now updated my routes from the file api.js in ‘routes’ directory to make use of the new model.

In Routes directory, I opened api.js with `vim api.js`, deleted the code inside with `:%d` command and wrote code below into it then save and exit

`const express = require ('express');

const router = express.Router();

const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client

Todo.find({}, 'action')

.then(data => res.json(data))

.catch(next)

});

router.post('/todos', (req, res, next) => {

if(req.body.action){

Todo.create(req.body)

.then(data => res.json(data))

.catch(next)

}else {

res.json({

error: "The input field is empty"

})

}

});


router.delete('/todos/:id', (req, res, next) => {

Todo.findOneAndDelete({"_id": req.params.id})

.then(data => res.json(data))

.catch(next)

})

module.exports = router;`

### MONGODB DATABASE

I need a database where I can store my data. For this i make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy.

In the index.js file, I specified process.env to access environment variables, but I have not yet created this file. So I will need to do that now.

so I created a file in my Todo directory and name it .env.

`touch .env`   `vi .env`

I added the connection string to access the database in it.

`DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'`

Ensure to update <username>, <password>, <network-address> and <database> according to your setup
  
Now I need to update the index.js to reflect the use of .env so that Node.js can connect to the database.

Simply I deleted the existing content in the file, and update it with the entire code below.

- Open the file with `vim index.js`
- Second item
- Third item









