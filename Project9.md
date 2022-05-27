# TOOLING WEBSITE DEPLOYMENT AUTOMATION WITH CONTINUOUS INTEGRATION. INTRODUCTION TO JENKINS

   ## Task to be executed and the architecture are giving below.
By enhance the architecture prepared in Project 8 I have to add Jenkins server, and i have to configured a job to automatically deploy source codes changes from Git to NFS server.

Here is the updated architecture and what it will look like upon competion of this project:
![Screenshot 2022-05-27 at 13 41 56](https://user-images.githubusercontent.com/96737660/170701289-a09799e2-3d61-4693-a304-d6400405cc8d.png)

## INSTALL AND CONFIGURE JENKINS SERVER STEPS ARE GIVING BELOW.

1. Spin up an AWS EC2 server based on Ubuntu Server 20.04 LTS and name it "Jenkins"
2. Installed JDK (since Jenkins is a Java-based application) with the command below.

        sudo apt update
        
        sudo apt install default-jdk-headless

Install Jenkins with the command below.

         wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
         sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
         /etc/apt/sources.list.d/jenkins.list'
         
         sudo apt update

         sudo apt-get install jenkins

I checked the Jenkins server if is up and running with the commmand below.

        sudo systemctl status jenkins
        
![Screenshot 2022-05-24 at 13 35 24](https://user-images.githubusercontent.com/96737660/170705535-cde0ebbe-5580-4360-b63e-6cc3629966fe.png)

By default Jenkins server uses TCP port 8080, I opened it by creating a new Inbound Rule in my EC2 Security Group

![Screenshot 2022-05-27 at 14 13 22](https://user-images.githubusercontent.com/96737660/170706258-64297385-46dd-427d-9248-4a91f34534ec.png)

From my browser access

http://<Jenkins-Server-Public-IP-Address-or-Public-DNS-Name>:8080
  
I was prompted to provide a default admin password
  
![Screenshot 2022-05-27 at 14 17 22](https://user-images.githubusercontent.com/96737660/170706958-46986f55-f563-4b88-a166-6c71fdc34fdc.png)

I retrieve the admin password from my server on the terminal with the command below:
  
         sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  
  after login with the password i was asked which plugings to install and i choose suggested plugins
  
![Screenshot 2022-05-27 at 14 24 00](https://user-images.githubusercontent.com/96737660/170708076-39529ba0-8c1c-453a-997f-d43a1697cccb.png)
  
![Screenshot 2022-05-24 at 13 39 10](https://user-images.githubusercontent.com/96737660/170708458-6e1943ca-73af-444b-92e2-029f68cecf1c.png)
  
  
![Screenshot 2022-05-24 at 13 42 59](https://user-images.githubusercontent.com/96737660/170725304-a0c786c0-5af3-4692-bd43-9d78ac611f32.png)

  ## Step 2 – Configure Jenkins to retrieve source codes from GitHub using Webhooks
  
This part, I configured a simple Jenkins job/project and the job was triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.
  
I enabled webhooks in my GitHub repository settings
  
![Screenshot 2022-05-27 at 14 37 54](https://user-images.githubusercontent.com/96737660/170710608-d1086e41-3e33-4bd6-b3e3-e11a7f6d577f.png)
  
Went into my Jenkins web console, click "New Item" and createed a "Freestyle project"
  
  ![Screenshot 2022-05-27 at 14 41 45](https://user-images.githubusercontent.com/96737660/170711240-7a193fd3-44a4-4751-a08e-358cb5ecf4a2.png)

I connected my GitHub repository, I have to provide my URL, which I copied from my repository.

In configuration of my Jenkins freestyle project I choosed Git repository, and provide there the link to my Tooling GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

![Screenshot 2022-05-27 at 14 49 31](https://user-images.githubusercontent.com/96737660/170712737-522221a4-fd36-4598-900f-f18502a0a573.png)

I Saved the configuration and I tried to run the build  manually.
I click on  "Build Now" button,and  the build was successfull and it showed  it under #1
  
And I opened the build and checked it in  "Console Output" it ran successfully
  
  ![Screenshot 2022-05-25 at 19 33 36](https://user-images.githubusercontent.com/96737660/170713877-4152b855-bb8e-4cde-8dea-20a04f8d8664.png)

But this build does not produce anything and it ran only when i triggered it manually. By fixed it.

I Clicked on  "Configure"  job/project and add these two configurations
Configure triggering the job from GitHub webhook:

![Screenshot 2022-05-27 at 15 05 11](https://user-images.githubusercontent.com/96737660/170715454-b47e77a2-db16-4c2d-a588-58335e794b73.png)

  I also configured "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".
  
  ![Screenshot 2022-05-27 at 15 07 31](https://user-images.githubusercontent.com/96737660/170715905-7c4ac743-b25c-4db5-b352-3b873c11ba7f.png)
  
I went ahead and make some change in my GitHub repository file which is the  README.MD file and push the changes to the master branch. and a new build was launched automatically by webhook and results was showed on the jenkins dashboard  – artifacts, saved on Jenkins server.
  
  ![Screenshot 2022-05-27 at 15 13 23](https://user-images.githubusercontent.com/96737660/170716821-b4915238-9a81-4d22-b4e9-09ea3e632523.png)

  By default, the artifacts are stored on Jenkins server locally and i checked with this command below.
  
         ls /var/lib/jenkins/jobs/tooling_github/builds/<build_number>/archive/
  
  ![Screenshot 2022-05-24 at 14 55 13](https://user-images.githubusercontent.com/96737660/170717694-be897ba2-fd7a-4e7f-84bb-443cd667f693.png)
  
  ## CONFIGURE JENKINS TO COPY FILES TO NFS SERVER VIA SSH
  
 Now I  have MY artifacts saved locally on Jenkins server, the next step for me was to copied them to our NFS server to /mnt/apps directory. and i have to installed  plugin that is called "Publish Over SSH". On the  main dashboard i selected "Manage Jenkins" and choose "Manage Plugins" menu item. On Available tab I searched for "Publish Over SSH" plugin and install it
  
  ![Screenshot 2022-05-27 at 15 30 36](https://user-images.githubusercontent.com/96737660/170720126-42963798-670e-4348-bfa5-2565d806339c.png)

I Configured the job/project to be able to copy artifacts over to NFS server.
On main dashboard I selected "Manage Jenkins" and I choosed "Configure System" menu item.

I also Scroll down to Publish over SSH plugin configuration section and I configure it to be able to connect to my NFS server:

I also Provided a private key (content of .pem file that i use to connect to NFS server via SSH)
Arbitrary name
The hostname i used was my private IP address of the NFS server
Username – ec2-user (since my NFS server is based on EC2 with RHEL 8)
Remote directory – /mnt/apps since my Web Servers use it as a mointing point to retrieve files from the NFS server
and I Tested the configuration and  the connection returns Success. i also opened  TCP port 22 on NFS server  to receive SSH connections.

![Screenshot 2022-05-27 at 15 40 08](https://user-images.githubusercontent.com/96737660/170721840-858e4167-1a51-496c-bd9c-44baa37abaa1.png)

 I Saved the configuration, opened my Jenkins job/project configuration page and add another one "Post-build Action"

![Screenshot 2022-05-27 at 15 42 13](https://user-images.githubusercontent.com/96737660/170722247-f3c1650c-aa0a-4400-ba52-b3205ef503ed.png)
  
I ConfigureD it to send all the files produced by the build into my previously define remote directory. I copy all files and directories – I have to used **.

  ![Screenshot 2022-05-27 at 15 46 15](https://user-images.githubusercontent.com/96737660/170723015-693108d3-ca91-4932-9a11-abb0029353b4.png)

I Saved this configuration and went ahead, to change something in the  README.MD file in my GitHub Tooling repository.

Webhook triggered a new job and i checked the "Console Output" of the job i  find something like this:
  
    SSH: Transferred 25 file(s)
  
    Finished: SUCCESS
  
To make sure that the files in my  /mnt/apps have been updated – i connected via SSH to my NFS server and check README.MD file with the command below.
  
    cat /mnt/apps/README.md
  
  ![Screenshot 2022-05-25 at 19 36 12](https://user-images.githubusercontent.com/96737660/170724132-0ca2ce7e-f9df-44c7-ad9e-5ea630954fdb.png)

