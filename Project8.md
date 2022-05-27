# LOAD BALANCER SOLUTION WITH APACHE

![Screenshot 2022-05-27 at 17 34 37](https://user-images.githubusercontent.com/96737660/170742955-acbd61ee-8ca5-4856-a52a-1b689b37c395.png)

              Task
I deployed and configured an Apache Load Balancer for Tooling Website solution on a separate Ubuntu EC2 intance. and also Make sure that users can be served by Web servers through the Load Balancer.

## CONFIGURE APACHE AS A LOAD BALANCER

      ###Steps are giving below.
   1. Spin up an Ubuntu Server 20.04 EC2 instance and named it Project-8-apache-lb,

![Screenshot 2022-05-27 at 17 57 39](https://user-images.githubusercontent.com/96737660/170744826-eca48c99-8ded-4688-8eac-46212be8c0be.png)

  2. I Opened TCP port 80 on my Project-8-apache-lb by creating an Inbound Rule in Security Group.

  3. Installed Apache Load Balancer on Project-8-apache-lb server and configured it to point traffic coming to LB to both Web Servers: with the command below.

                sudo apt update
         
 ![Screenshot 2022-05-18 at 18 29 11](https://user-images.githubusercontent.com/96737660/170748425-7129d26d-3f63-45a7-8877-8ae0f5be62b1.png)

              sudo apt install apache2 -y
           
![Screenshot 2022-05-18 at 18 29 57](https://user-images.githubusercontent.com/96737660/170748891-1f9325f1-b041-4e0b-b7a0-62b6ba784acd.png)

           
          sudo apt-get install libxml2-dev

Enable following modules on the httpd server
       sudo a2enmod rewrite
       
       sudo a2enmod proxy
      
       sudo a2enmod proxy_balancer
     
       sudo a2enmod proxy_http
      
       sudo a2enmod headers
      
       sudo a2enmod lbmethod_bytraffic
    
![Screenshot 2022-05-18 at 18 41 27](https://user-images.githubusercontent.com/96737660/170751586-37942ef6-21fd-450a-9cfd-9ca2d35b102f.png)

I Restart apache2 service with the command below.

        sudo systemctl restart apache2
      
  Checked with the command below to see if  apache2 is up and running
  
          sudo systemctl status apache2
          
![Screenshot 2022-05-18 at 18 45 38](https://user-images.githubusercontent.com/96737660/170755967-45d26148-a5f4-45bc-baa5-b808255d6730.png)

I configured load balancing with the code below.

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
    
   I Opened two ssh consoles for both Web Servers and run following command below:
   
            sudo tail -f /var/log/httpd/access_log
            
  ![Screenshot 2022-05-18 at 19 18 43](https://user-images.githubusercontent.com/96737660/170763463-ed4dd28d-dffb-4dab-bde1-31525a83b8a9.png)

I Verified that my configuration worked – by trying to access my LB’s public IP address or Public DNS name from my browser:

            http://18.130.230.24/index.php
            
 ![Screenshot 2022-05-18 at 17 49 24](https://user-images.githubusercontent.com/96737660/170762168-3b438a8a-0175-435a-a41f-d66d2d51f386.png)
 
 I login with my username and password
 
 ![Screenshot 2022-05-18 at 17 47 04](https://user-images.githubusercontent.com/96737660/170762320-b3999386-409a-4838-a020-7623bb7730cd.png)
 
I configured local domain name resolution. and the easiest way for me is that i  used /etc/hosts file, although this approach is not very scalable, but it is very easy to configure and shows the concept well. So I configureD IP address to domain name mapping for our LB.

I opened this file on my LB server with command below.

       sudo vi /etc/hosts
       
![Screenshot 2022-05-18 at 19 40 52](https://user-images.githubusercontent.com/96737660/170764792-40f27c64-4ec9-4230-9084-e93a950da693.png)

Add 2 records into this file with Local IP address and arbitrary name for both of my Web Servers

           <WebServer1-Private-IP-Address> Web1
          <WebServer2-Private-IP-Address> Web2
  
  I updated my LB config file with those names instead of IP addresses.
  
      BalancerMember http://Web1:80 loadfactor=5 timeout=1
      BalancerMember http://Web2:80 loadfactor=5 timeout=1
      
      after the all configuration I tried to curl my Web Servers from LB locally curl http://Web1 or curl http://Web2 –  and it  worked.
      
 ![Screenshot 2022-05-18 at 19 41 50](https://user-images.githubusercontent.com/96737660/170765501-5efcca16-9ce4-4e00-be89-e212a5c393be.png)

## Targed Architecture

After the all project implementation this what my set up looked like below

![Screenshot 2022-05-27 at 19 04 28](https://user-images.githubusercontent.com/96737660/170766402-c6b52188-cb0c-4ccf-9b8c-494846828dea.png)

