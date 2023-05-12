# AWESOME DOCUMENTATION OF PROJECT 2

## **INSTALLING THE NGINX WEB SERVER**

i installed the package with the command

`sudo apt update`

`sudo apt install nginx`

To verify that nginx was successfully installed and is running as a service in Ubuntu, I run:

`sudo systemctl status nginx`

<img width="518" alt="active nginx" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/4ae50f2f-fa26-4b9a-b4d4-2282d7a19200">

i added a new rule to my EC2

Then i try to access it locally in my Ubuntu shell with the command

`curl http://localhost:80`

<img width="515" alt="curl " src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/7fa0bdc5-538a-4521-8e35-6dbdf1946b65">

Then I tested ny nginx server on the internet by entering my public address on my browser

<img width="715" alt="nginx welcome" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/7f472e25-28e4-4705-b4b5-83f416a7977f">

## INSTALLING MYSQL

`sudo apt install mysql-server`

When the installation is finished, log in to the MySQL console by typing:

`sudo mysql`

<img width="520" alt="mysql" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/92453cc0-a786-4b4b-9782-005ff93aa9d6">

I set a password for the root user, using mysql_native_password as default authentication method.

I started the interactive script by running:

`sudo mysql_secure_installation`

Then I tested if am able to log in to the MySQL console by typing:

`sudo mysql -p`

## INSTALLING PHP

I installed these 2 packages at once, by runing:

`sudo apt install php-fpm php-mysql`

after it was installed, I now configured nginx to use them

i used projectLEMP as a domain name.

I created the root web directory for my domain as follows:

`sudo mkdir var/www/projectLEMP`

Next, I assign ownership of the directory with the $USER environment variable, which will reference my current system user:

`sudo chown -R $USER:$USER /var/www/projectLEMP`

Then, I opened a new configuration file in Nginx’s sites-available directory using nano command-line editor:

<img width="520" alt="projectLEMP file" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/08524e6f-4cbd-4c70-ae44-d474096493c2">

I activated the configuration by linking to the config file from Nginx’s sites-enabled directory, then tested for syntax error

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

`sudo nginx -t`

I also need to disable default Nginx host that is currently configured to listen on port 80, with the command:

`sudo unlink /etc/nginx/sites-enabled/default`

I reloaded Nginx to apply the changes:

`sudo systemctl reload nginx`

I created an index.html file in projectLEMP location so that I can test that my new server block works as expected:

<img width="603" alt="html file" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/2bd328d8-9ef9-4cfd-8014-45a291ec293d">

## TESTING PHP WITH NGINX

I created a test PHP file in my document root. I Opened a new file called info.php within my document root in my text editor:

`sudo nano /var/www/projectLEMP/info.php`

<img width="518" alt="php file1" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/28984f2b-130d-4253-ba5b-a71a5d10a4bf">

i went ahead to access my page on my web browser to check.

<img width="781" alt="php info" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/150ab943-924e-4d15-8746-30dae9d36778">

I removed the php file i created because it contains sensitive information, with the command:

`sudo rm /var/www/my_domain/info.php`

## RETRIEVING DATA FROM MYSQL DATABASE WITH PHP

I created a new user with the mysql_native_password authentication method in order to be able to connect to my MySQL database from PHP.

I created a database named ope_database and a user named ope_user

First, I connected to the MySQL console using the root account:

`sudo mysql -p`

I created a new database, with the command from your MySQL console:

`mysql> CREATE DATABASE `ope_database`;`

I created a new user and grant him full privileges on the database I have just created.

`mysql>  CREATE USER 'ope_user'@'%' IDENTIFIED WITH mysql_native_password BY 'myown_password';`

Now I gave this ope_user permission over the ope_database database:

`mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';`

I tested if my new user has the proper permissions by logging in to my MySQL console again, this time using the custom user credentials:

`mysql -u ope_user -p`

I now confirmed I have access to the example_database database:

`mysql> SHOW DATABASES;`

<img width="515" alt="database" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/ebb5eb41-e5e3-48a0-bf16-882dca40c201">

Next, I created a test table named todo_list. From my MySQL console:

`CREATE TABLE ope_database.todo_list (

mysql>     item_id INT AUTO_INCREMENT,

mysql>     content VARCHAR(255),

mysql>     PRIMARY KEY(item_id)

mysql> );`

Insert a few rows of content in the test table:

`mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");`

I confirmed that the data was successfully saved to my table.

`mysql>  SELECT * FROM example_database.todo_list;`

<img width="524" alt="todo list 1" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/64faa960-06fa-4cec-a93c-a91d32f05567">

i exit my mysql

I created a new PHP file in my custom web root directory using your preferred editor.

`nano /var/www/projectLEMP/todo_list.php`

`<?php

$user = "ope_user";

$password = "my_password";

$database = "ope_database";

$table = "todo_list";

try {

  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  
  echo "<h2>TODO</h2><ol>";
  
  foreach($db->query("SELECT content FROM $table") as $row) {
  
    echo "<li>" . $row['content'] . "</li>";
    
  }
  echo "</ol>";
  
} catch (PDOException $e) {

    print "Error!: " . $e->getMessage() . "<br/>";
    
    die();
    
}`


 I now access my page in my web browser by visiting the my domain name or public IP address configured for my website, followed by /todo_list.php:
 
 `http://<Public_domain_or_IP>/todo_list.php`
 
 <img width="359" alt="final proj2 result" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6646787f-77d8-4d65-85d1-32affb123fa6">

 
 And that brings me to the end of project 2 documentation.










