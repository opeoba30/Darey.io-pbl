# LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

Aside implementing the load balaning solution with apache

## Configure Nginx As A Load Balancer

1. Create an Nginx WebServer which will be configured as loadbalancer

<img width="785" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/e1839aac-9995-4187-ad6d-c5d0a0138eab">

2. Update /etc/hosts file for local DNS with Web Servers’ names (e.g. Web1 and Web2) and their local IP addresses

<img width="269" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/793fc4c1-94e4-4661-a619-3507a9d14d11">

3. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers

Update the instance and Install Nginx

`sudo apt update`
`sudo apt install nginx`

Configure Nginx LB using Web Servers’ names defined in /etc/hosts

Open the default nginx configuration file

sudo vi /etc/nginx/nginx.conf

insert the code in the image below in the http block

<img width="315" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/a4b64256-035c-4e98-b735-aa0a0aca5535">

Restart Nginx and make sure the service is up and running

`sudo systemctl restart nginx`
`sudo systemctl status nginx`  

<img width="342" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/49121e13-36c3-41dd-b30b-162d8d390ffc">


## REGISTER A NEW DOMAIN NAME AND CONFIGURE SECURED CONNECTION USING SSL/TLS CERTIFICATES

In order to get a valid SSL certificate – you need to register a new domain name, you can do it using any Domain name registrar – a company that manages reservation of domain names. The most popular ones are: Godaddy.com, Domain.com, Bluehost.com.

1. Register a new domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other)
2. Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP

<img width="748" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/308472e8-a824-4017-a3ab-9f8fd662e6db">

<img width="752" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/5fc0159a-c7d7-4f17-9077-f3cb292c68c8">

3. Update A record in your registrar to point to Nginx LB using Elastic IP address

   <img width="493" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/98485bf5-1cbc-4ba6-bd0b-5f18eef21307">

Check that your Web Servers can be reached from your browser using new domain name using HTTP protocol – http://<your-domain-name.com>

<img width="922" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6f624090-0364-442f-9ce3-820df8e401fc">

4. Configure Nginx to recognize your new domain name

Update your nginx.conf with server_name www.<your-domain-name.com> instead of server_name www.domain.com

<img width="202" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/6e7a5696-891f-40a7-b668-256502211391">

`sudo systemctl restart nginx`

5. Install certbot and request for an SSL/TLS certificate

Make sure snapd service is active and running

`sudo systemctl status snapd`

<img width="341" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/61ac548f-3a36-461b-b544-7e3095882f53">

Install certbot

`sudo snap install --classic certbot`

Request your certificate (just follow the certbot instructions – you will need to choose which domain you want your certificate to be issued for, domain name will be looked up from nginx.conf file so make sure you have updated it on step 4).

`sudo ln -s /snap/bin/certbot /usr/bin/certbot`
`sudo certbot --nginx`

<img width="343" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/584c63c2-e576-484a-98d5-42ebfb3a84c0">

Test secured access to your Web Solution by trying to reach https://<your-domain-name.com>

You shall be able to access your website by using HTTPS protocol (that uses TCP port 443) and see a padlock pictogram in your browser’s search string.
Click on the padlock icon and you can see the details of the certificate issued for your website.     

<img width="781" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/4d4176b3-1882-4f07-bc00-6e27756b2542">


6. Set up periodical renewal of your SSL/TLS certificate

By default, LetsEncrypt certificate is valid for 90 days, so it is recommended to renew it at least every 60 days or more frequently.

You can test renewal command in dry-run mode

`sudo certbot renew --dry-run`

<img width="295" alt="image" src="https://github.com/opeoba30/Darey.io-pbl/assets/132816403/3773a624-b0aa-4043-9a5c-d0c9d8e0df5f">

Best practice is to have a scheduled job that to run renew command periodically. Let us configure a cronjob to run the command twice a day.

To do so, lets edit the crontab file with the following command:

`sudo vi crontab -e`

Add following line:

`* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1`

You can always change the interval of this cronjob if twice a day is too often by adjusting schedule expression.

Congratulations!
You have just implemented an Nginx Load Balancing Web Solution with secured HTTPS connection with periodically updated SSL/TLS certificates.

![thank you](https://github.com/opeoba30/Darey.io-pbl/assets/132816403/5b7ffb76-13bd-48f2-956f-431473bdc255)




