# LOAD BALANCER SOLUTION WITH APACHE

After completing Devops Tooling Website Solution (project 7) you might wonder how a user will be accessing each of the webservers using 3 different IP addreses or 3 different DNS names. You might also wonder what is the point of having 3 different servers doing the same thing.

In order to hide all this complexity and to have a single point of access with a single public IP address/name, a Load Balancer can be used. A Load Balancer (LB) distributes clients’ requests among underlying Web Servers and makes sure that the load is distributed in an optimal way.


## Task

Deploy and configure an Apache Load Balancer for Tooling Website solution on a separate Ubuntu EC2 intance. Make sure that users can be served by Web servers through the Load Balancer.

To simplify, let us implement this solution with 2 Web Servers, the approach will be the same for 3 and more Web Servers

## Prerequisites

Make sure that you have following servers installed and configured within Project-7: (i.e DEVOPS TOOLING WEBSITE SOLUTION)

1.	Two RHEL8 Web Servers
2.	One MySQL DB Server (based on Ubuntu 20.04)
3.	One RHEL8 NFS server

## CONFIGURE APACHE AS A LOAD BALANCER
1.	Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb, so your EC2 list will look like this:

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/1a0ef252-b98c-45f0-b1ae-dfa952223818)

2.	Open TCP port 80 on Project-8-apache-lb by creating an Inbound Rule in Security Group.

3.	Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both Web Servers:

        #Install apache2
        sudo apt update
        sudo apt install apache2 -y
        sudo apt-get install libxml2-dev
        
        #Enable following modules:
        sudo a2enmod rewrite
        sudo a2enmod proxy
        sudo a2enmod proxy_balancer
        sudo a2enmod proxy_http
        sudo a2enmod headers
        sudo a2enmod lbmethod_bytraffic
        
        #Restart apache2 service
        sudo systemctl restart apache2
  	
  	Make sure apache2 is up and running

          sudo systemctl status apache2

   ![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/89491a21-6bd3-46e9-8aed-08ad3161311b)


   Configure load balancing

      sudo vi /etc/apache2/sites-available/000-default.conf
      
      #Add this configuration into this section <VirtualHost *:80>  </VirtualHost>
      
      <Proxy "balancer://mycluster">
                     BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
                     BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
                     ProxySet lbmethod=bytraffic
                     # ProxySet lbmethod=byrequests
              </Proxy>
      
              ProxyPreserveHost On
              ProxyPass / balancer://mycluster/
              ProxyPassReverse / balancer://mycluster/
      
      #Restart apache server
      
      sudo systemctl restart apache2   
      
![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/9d7bc51c-48f2-45bc-994b-a847fc804155)

bytraffic balancing method will distribute incoming load between your Web Servers according to current traffic load. We can control in which proportion the traffic must be distributed by loadfactor parameter.

4.	Verify that our configuration works – try to access your LB’s public IP address or Public DNS name from your browser:

        http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php

   ![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/16f2bb15-a327-4a0b-96a3-a6eece699037)

Note: If in the Project-7 you mounted /var/log/httpd/ from your Web Servers to the NFS server – unmount them and make sure that each Web Server has its own log directory.

Open two ssh/Putty consoles for both Web Servers and run following command:

`sudo tail -f /var/log/httpd/access_log`

Try to refresh your browser page http://<Load-Balancer-Public-IP-Address-or-Public-DNS-Name>/index.php several times and make sure that both servers receive HTTP GET requests from your LB – new records must appear in each server’s log file. The number of requests to each server will be approximately the same since we set loadfactor to the same value for both servers – it means that traffic will be disctributed evenly between them.

If you have configured everything correctly – your users will not even notice that their requests are served by more than one server.

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/4cafccca-8a49-4916-b3ff-e6ce16a41d76)

### Optional Step – Configure Local DNS Names Resolution

Sometimes it is tedious to remember and switch between IP addresses, especially if you have a lot of servers under your management.
What we can do, is to configure local domain name resolution. The easiest way is to use /etc/hosts file, although this approach is not very scalable, but it is very easy to configure and shows the concept well. So let us configure IP address to domain name mapping for our LB.

      #Open this file on your LB server
      
      sudo vi /etc/hosts
      
      #Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers
      
      <WebServer1-Private-IP-Address> Web1
      <WebServer2-Private-IP-Address> Web2

Now you can update your LB config file with those names instead of IP addresses.

      BalancerMember http://Web1:80 loadfactor=5 timeout=1
      BalancerMember http://Web2:80 loadfactor=5 timeout=1

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/3ac4eb9d-5c35-4bd2-a3a7-6af012d5e943)

You can try to curl your Web Servers from LB locally `curl http://Web1` or `curl http://Web2` – it shall work.

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/7003dd41-a2f3-4999-9b0b-b5740920ddd3)

Remember, this is only internal configuration, and it is also local to your LB server, these names will neither be ‘resolvable’ from other servers internally nor from the Internet.

Targed Architecture
Now your set up looks like this:

![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/50013d01-9950-4a71-950d-d5357bfb2221)


## Congratulations!

We have just implemented a Load balancing Web Solution for your DevOps team


![image](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/d8bd38e2-5557-4fa1-86ab-f88d66b193d4)






