# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
## LINES OF COMMOND AND IMAGES
cd ~/Downloads
sudo chmod 0400 <private-key-name>.pem
ssh -i <private-key-name>.pem ubuntu@<Public-IP-addres>
![Screenshot 2022-03-27 at 20 57 37](https://user-images.githubusercontent.com/96737660/160538879-e770e4b6-965c-4a90-8e32-b26cb2c1a0cb.png)
update a list of packages in package manager sudo apt update
run apache2 package installation sudo apt install apache2 
sudo systemctl status apache2
![Screenshot 2022-03-27 at 21 02 14](https://user-images.githubusercontent.com/96737660/160539425-3823e419-58c7-4abb-bb31-5c401d5d9d4b.png)
curl http://localhost:80
  http://<Public-IP-Address>:80
![Screenshot 2022-03-27 at 21 47 17](https://user-images.githubusercontent.com/96737660/160539735-ac9a9e67-08ce-4dca-bf3e-b97fd1f9a171.png)
sudo apt install mysql-server
sudo mysql_secure_installation
sudo mysql
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
sudo vi /etc/apache2/sites-available/projectlamp.conf
sudo systemctl reload apache2
sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
http://<Public-IP-Address>:80
![Screenshot 2022-03-27 at 22 27 30](https://user-images.githubusercontent.com/96737660/160541200-e5cdf055-8957-4f90-ab58-ff59d043d970.png)
sudo vim /etc/apache2/mods-enabled/dir.conf
  <IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
sudo systemctl reload apache2
vim /var/www/projectlamp/index.php
  <?php
phpinfo();
![Screenshot 2022-03-28 at 18 18 24](https://user-images.githubusercontent.com/96737660/160541779-50fc6bdb-dd2f-45f0-95f1-26b6f708c0d7.png)
sudo rm /var/www/projectlamp/index.php
