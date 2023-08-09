# IMPLEMENTING A CLIENT-SERVER ARCHITECTURE USING MYSQL DATABASE MANAGEMENT SYSTEM ON AWS
---

## What is a Client/Server Architecture?

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/Client_Server_Architecture_1.webp)

Client-server architecture is a computer network architecture in which many clients (remote processors) request and receive service from a centralized server (host computer). Client computers provide an interface to allow a computer user to request services of the server and to display the results the server returns. Servers wait for requests to arrive from clients and then respond to them. Ideally, a server provides a standardized transparent interface to clients so that clients need not be aware of the specifics of the system (i.e., the hardware and software) that is providing the service. Clients are often situated at workstations or on personal computers, while servers are located elsewhere on the network, usually on more powerful machines. 

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/Client-server2.png)

From the picture above, our Web Server has a role of a “Client” that connects and reads/writes to/from a Database (DB) Server (MySQL, MongoDB, Oracle, SQL Server, or any other). The communication between them happens over a Local Network (it can also be Internet connection, but it is a common practice to place Web Server and DB Server close to each other in local network).

For this project, we are provisioning two Linux-based virtual servers (EC2 instances in AWS). 
* Server A name - `mysql server`
* Server B name - `mysql client`
Both of our EC2 servers are located in the same local network, so we will connect them using their respective Ip Address and have them send and return requests between each-other.

## Connecting to the EC2 servers through SSH

> NB: For this project, I already created the EC2 server on AWS, I need to connect to it to start the implementation. I also used two terminals, one for the Server and one for the Client.

``` ssh -i "mykey.pem" ubuntu@ec2-184-72-73-85.compute-1.amazonaws.com ```

```ssh -i "mykey.pem" ubuntu@ec2-52-90-160-253.compute-1.amazonaws.com```


Connected Successfully to the MySQL server
![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/connect%201.JPG)

Connected Successfully to MySQL client 
![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/Connect%202.JPG)

### Installing MySQL Server software on the mysql server EC2 instance

We installed the mysql server with the command:

```sudo apt install mysql-server```

To check the installation status we use the command

```sudo service mysql status```

And we have this, which shows the installation is successful.
![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/mysql%20status.JPG)

### Installing MySQL Client software on mysql client EC2 instance

We installed the mysql client with the command:

```sudo apt install mysql-client```

By default, both of our EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses. We use mysql server's local IP address to connect from mysql client. MySQL server uses TCP port 3306 by default, so we must open it by creating a new entry in ‘Inbound rules’ in ‘mysql server’ Security Groups. For extra security, we do not allow all IP addresses to reach our ‘mysql server’. We only allow access to the specific local IP address of our ‘mysql client’.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/security%20group.JPG)

## Configuring MySQL Server to allow connection from remote hosts.

To configure MySQL server to allow connections from remote hosts, we use vim to open the mysqld.cnf file with this command:

```sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf```

And we search for the bind-address and Replace ‘127.0.0.1’ to ‘0.0.0.0’ like this: 

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/Bind.JPG)

## Creating a remote user on MySQL Server.

After configuring mysql server to allow connections from remote hosts, we need to create a remote user that can access our database from remote hosts. to do this, we first connect to our mysql database on the mysql server EC2 instance:

```sudo mysql```

We can list all users in our database with the command:

```select user,plugin,host from mysql.user;```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/old%20users.JPG)

This displays the list of all users that can access our database. To create a new remote user:

```CREATE USER 'toluwase'@'%' IDENTIFIED WITH mysql_native_password BY 'toluwase123';```

The % sign will allow connections from any remote IP address. toluwase is the username and the password is toluwase123 as shown in the command.

To verify our new user is created, we run the command:

```select user,plugin,host from mysql.user;```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/new%20user.JPG)

And we can see that user toluwase is now created.

## Connecting to database from MySQL Client EC2 instance.

After creating the remote user, we can now head to the mysql client EC2 instance shell and connect to the database by using the command:

```mysql --host=172.31.83.19 --user=toluwase --password=toluwase123```

> NB: The IP address in the command is the private IP address of the MySQL Server EC2 instance.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/connected.JPG)

The connection was successful. We can now start running queries on the database:

```show databases;```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%205/show%20databases.JPG)

With this, we have successfully implemented a client-server architecture using MySQL DBMS on AWS.





