# LOAD BALANCER SOLUTION WITH NGINX AND SSL/TLS

Task to be excuted in this project is giving below.

The project consists of two parts:

Configuration of  Nginx as a Load Balancer

Registration of domain name and configuration secured connection using SSL/TLS certificates

My target architecture is giving below:

![Screenshot 2022-05-27 at 11 39 20](https://user-images.githubusercontent.com/96737660/170684297-ea0ac6c1-4ddb-415f-be4f-a7671335e266.png)

## CONFIGURE NGINX AS A LOAD BALANCER

         steps are giving below.
    
spin up an EC2 VM based on Ubuntu Server 20.04 LTS 

Name it Nginx LB

I opened TCP port 80 for HTTP connections, also opened TCP port 443 which is used for secured HTTPS connections.

I UpdateD /etc/hosts file for the local DNS with Web Servers’ names ( Web1, Web2 and web3) their their local IP addresses.

Installed and configured Nginx as a load balancer to point traffic to the resolvable DNS names of the webservers with the command below.

           sudo apt update

           sudo apt install nginx

Opened the default nginx configuration file with the command below.

          sudo vi /etc/nginx/nginx.conf
     
       #insert following configuration into http section

       upstream myproject {
        server Web1 weight=5;
        server Web2 weight=5;
        server web3 weight=5;
      }

       server {
       listen 80;
       server_name www.practicelab.link;
       location / {
        proxy_pass http://myproject;
       }
    }

      #comment out this line
          # include /etc/nginx/sites-enabled/*;

Restart Nginx with the command below to checked if  the service is up and running.
     
          sudo systemctl restart nginx
     
          sudo systemctl status nginx
     
![Screenshot 2022-05-25 at 11 55 03](https://user-images.githubusercontent.com/96737660/170690651-3289eb8a-9960-4a4e-92d5-5c32f17aa068.png)

Checked my Web Servers to see if it can be reached from the browser with my  domain name using HTTP protocol
http://practicelab.link

![Screenshot 2022-05-26 at 18 39 13](https://user-images.githubusercontent.com/96737660/170691943-bb37304d-6763-405b-b703-25afbae11399.png)

Installed certbot and request for an SSL/TLS certificate with the command below.

        sudo snap install --classic certbot
   
        sudo systemctl status snapd
   
![Screenshot 2022-05-26 at 19 31 38](https://user-images.githubusercontent.com/96737660/170692477-bb3b8183-a522-41c2-892c-911e514be1be.png)

Request a certificate the certbot and followed the instructions prompt up from the terminal and  choose the domain i want my  certificate to be issued for, and the domain name was looked up from nginx.conf file which i  have updated with the command below.

        sudo ln -s /snap/bin/certbot /usr/bin/certbot
     
        sudo certbot --nginx
     
![Screenshot 2022-05-26 at 19 35 37](https://user-images.githubusercontent.com/96737660/170693365-f3d17af2-a025-446c-8a3b-35423b89a92d.png)

I tested secured access to my Web Solution by trying to reach from the browser.
![Screenshot 2022-05-26 at 19 40 01](https://user-images.githubusercontent.com/96737660/170693626-c76afd95-51b6-4139-9498-b5657c928fff.png)

Tested the renewal command in dry-run mode

       sudo certbot renew --dry-run
   
Scheduled the job to run renew command periodically with the command below.

       crontab -e
     
![Screenshot 2022-05-26 at 19 55 46](https://user-images.githubusercontent.com/96737660/170694648-fb5fbdb6-8895-4e99-9737-0f24bf5842af.png)

Added the following line of command below.

       * */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1
