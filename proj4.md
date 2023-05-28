# MEAN STACK DEPLOYMENT TO UBUNTU IN AWS

having launch my instance on aws, i then ran the following on my Terminal 

`sudo apt update`

`sudo apt upgrade -y`

`sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates`

`curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

`sudo apt install -y nodejs`

# Install MongoDB

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6`

`echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

`sudo apt install -y mongodb`

`sudo service mongodb start`

`sudo systemctl status mongodb`

# Installing npm

`sudo apt-get install aptitude`

`sudo aptitude install npm`

`sudo npm install body-parser`
