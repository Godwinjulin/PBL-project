## Client/Server Architecture Using A MySQL Relational Database Management System

Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.

In their communication, each machine has its own role: the machine sending requests is usually referred as "Client" and the machine responding (serving) is called "Server".

A simple diagram of Web Client-Server architecture is presented below:

![Screenshot 2022-05-11 at 05 04 47](https://user-images.githubusercontent.com/96737660/167766988-782c96ee-9178-43db-a8fe-f259d2e6b4d4.png)

In the example above, a machine that is trying to access a Web site using Web browser or simply ‘curl’ command is a client and it

sends HTTP requests to a Web server (Apache, Nginx, IIS or any other) over the Internet.

If we extend this concept further and add a Database Server to our architecture, we can get this picture:

![Screenshot 2022-05-11 at 05 14 25](https://user-images.githubusercontent.com/96737660/167767923-44407f3c-039c-4bd1-8e60-494ed8a01ca9.png)

In this case, our Web Server has a role of a "Client" that connects and reads/writes to/from a Database (DB) Server (MySQL, 

MongoDB, Oracle, SQL Server or any other), and the communication between them happens over a Local Network (it can also be Internet 

connection, but it is a common practice to place Web Server and DB Server close to each other in local network).

The setup on the diagram above is a typical generic Web Stack architecture that I have already implemented in previous projects 

(LAMP, LEMP, MEAN, MERN), this architecture can be implemented with many other technologies – various Web and DB servers, from 

small Single-page applications SPA to large and complex portals.

## Real example of LAMP website
In Project 1 I implemented a LAMP STACK website, where by I take an example of commercially deployed LAMP website – www.propitixhomes.com.

And this LAMP website server(s) can be located anywhere in the world and you can reach it also from any part of the globe over global network – Internet.

Assuming that you go on your browser, and typed in there www.propitixhomes.com. It means that your browser is considered the "Client". Essentially, it is sending request to the remote server, and in turn, would be expecting some kind of response from the remote server.

By taking a very quick example and see Client-Server communicatation in action.

I opened up my Ubuntu or Windows terminal and run curl the command below.

     curl -Iv www.propitixhomes.com
     
In this example, my terminal was the client, while www.propitixhomes.com was the server.

See the response from the remote server in below output. I can also see that the requests from the URL are being served by a 

computer with an IP address 160.153.133.153 on port 80.

![Screenshot 2022-05-11 at 05 37 10](https://user-images.githubusercontent.com/96737660/167770157-22f21e73-d9e1-4f5a-bf9c-b892010488fd.png)

And another simple way to get a server’s IP address is to use a simple diagnostic tool like ‘ping’, it will also show round-trip time – 

time for packets to go to and back from the server, this tool uses ICMP protocol.

# IMPLEMENT A CLIENT SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM (DBMS).

TASK – Implement a Client Server Architecture using MySQL Database Management System (DBMS).
To demonstrate a basic client-server using MySQL Relational Database Management System (RDBMS), the instruction is give below

spin up two Linux-based virtual servers (EC2 instances in AWS).

Server A name - `mysql server`

Server B name - `mysql client`


I updated the two server with the command below

    sudo apt update -y

![update](https://user-images.githubusercontent.com/96737660/167765601-33dc8d17-ec35-4fde-b8d4-e75b70c1cee0.png)

On mysql server Linux Server install MySQL Server software with the command below.

     sudo apt install mysql-server -y
     
![mysql-server](https://user-images.githubusercontent.com/96737660/167765867-616c90cf-2b71-4c07-b59e-086aa84b09d5.png)


On mysql client Linux Server I install MySQL Client software with the command below.

    sudo apt install mysql-client -y

![mysql-client](https://user-images.githubusercontent.com/96737660/167770749-bf6e6eaf-182d-4a0a-a659-2d65e9f89f43.png)


By default, both of my EC2 virtual servers were located in the same local virtual network, so that they can communicate to each other 

using local IP addresses. and I Use mysql server's local IP address to connect from mysql client. And MySQL server uses TCP port 3306 by 

default, and  have to open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra 

security, and also I do not allow all IP addresses to reach my ‘mysql server’ – I only allow access only to the specific local IP address of my ‘mysql client’.

I also configure MySQL server to allow connections from remote hosts with this command below.

     sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
     
     Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this:     
     
![sss](https://user-images.githubusercontent.com/96737660/167771924-e2687c4c-e84d-42f6-b670-78a33097cb4d.png)

From mysql client Linux Server I connected remotely to mysql server Database Engine without using SSH.

And I also check that I have successfully connected to a remote MySQL server and I can perform SQL queries:

By using this command below

      Show databases;
      
![show database](https://user-images.githubusercontent.com/96737660/167772404-1b272d56-864a-45c8-beca-4f2023d8b5aa.png)
