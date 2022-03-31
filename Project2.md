# WEB STACK IMPLEMENTATION (LEMP STACK)
## STEP 1 – INSTALLING THE NGINX WEB SERVER
### COMMAND LINES
ssh -i <Your-private-key.pem> ubuntu@<EC2-Public-IP-address>
![Screenshot 2022-03-27 at 20 57 37](https://user-images.githubusercontent.com/96737660/161052625-f6ca98b7-a0ab-4a7e-8924-38f519b5ec81.png)
sudo apt update
sudo apt install nginx 
sudo systemctl status nginx
![Screenshot 2022-03-30 at 15 41 46](https://user-images.githubusercontent.com/96737660/161053175-d18511cd-151e-4bdc-a1e7-a85df5a92c10.png
curl http://localhost:80
or
curl http://127.0.0.1:80
http://<Public-IP-Address>:80
![Screenshot 2022-03-30 at 18 44 55](https://user-images.githubusercontent.com/96737660/161053629-d0cc555f-d826-43d9-a228-d8d959cce007.png)
  


 ## STEP 2 — INSTALLING MYSQL
sudo apt install mysql-server
sudo mysql_secure_installation
![Screenshot 2022-03-30 at 19 02 13](https://user-images.githubusercontent.com/96737660/161055710-dd05bbdb-1e3e-4679-9ada-3fd8157dfdfc.png)

 
  
  ## STEP 3 – INSTALLING PHP
  sudo apt install php-fpm php-mysql
  sudo mkdir /var/www/projectLEMP
  sudo chown -R $USER:$USER /var/www/projectLEMP
  sudo nano /etc/nginx/sites-available/projectLEMP
  /etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/ 
sudo nginx -t  
sudo unlink /etc/nginx/sites-enabled/default  
sudo systemctl reload nginx  
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html  
 http://<Public-IP-Address>:80  
![Screenshot 2022-03-30 at 19 25 39](https://user-images.githubusercontent.com/96737660/161057927-03c0d911-5b0d-48cc-b638-8a05cf8e7129.png)
  
 
  
  ## STEP 5 – TESTING PHP WITH NGINX  
 sudo nano /var/www/projectLEMP/info.php 
<?php
phpinfo();  
http://`server_domain_or_IP`/info.php
![Screenshot 2022-03-30 at 19 46 54](https://user-images.githubusercontent.com/96737660/161058496-0f22cc40-0bfd-4af1-8ff3-5bf55d6f9da5.png)
sudo rm /var/www/your_domain/info.php



## STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)
sudo mysql
![Screenshot 2022-03-30 at 18 55 04](https://user-images.githubusercontent.com/96737660/161059678-91aee773-e34e-456b-8da1-7081c2dd0b52.png)
mysql> CREATE DATABASE `brinisolution_database`
mysql>  CREATE USER 'brini20'@'%' IDENTIFIED WITH mysql_native_password BY 'Bridget20.user'; 
mysql> GRANT ALL ON brinisolution_database.* TO 'brini20'@'%';
mysql> exit
mysql -u example_user -p
mysql> SHOW DATABASES;
![Screenshot 2022-03-31 at 11 20 20](https://user-images.githubusercontent.com/96737660/161060994-318a6897-f672-4c9a-a754-fed9f58c7e5c.png)
CREATE TABLE brinisolution_database.todo_list (
mysql>     item_id INT AUTO_INCREMENT,
mysql>     content VARCHAR(255),
mysql>     PRIMARY KEY(item_id)
mysql> );
mysql> INSERT INTO brinisolution_database.todo_list (content) VALUES ("My first important item");
mysql>  SELECT * FROM brinisolution_database.todo_list;
![Screenshot 2022-03-31 at 11 36 45](https://user-images.githubusercontent.com/96737660/161061862-07118f54-0a5c-418f-b09e-637e34106cff.png)
mysql> exit
nano /var/www/projectLEMP/todo_list.php
<?php
$user = "brini20";
$password = "Bridget20.user";
$database = "brinisolution_database";
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
}
http://<Public_domain_or_IP>/todo_list.php
![Screenshot 2022-03-31 at 11 54 52](https://user-images.githubusercontent.com/96737660/161062436-f068678b-7b57-412e-923d-721837916848.png)



