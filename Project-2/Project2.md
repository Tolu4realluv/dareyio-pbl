# WEB STACK IMPLEMENTATION (LEMP STACK) IN AWS
---

## What is a Technology stack?
---
A stack is an arrangement of “things” kept in order one over the other. A technology stack is a set of technologies that are stacked together to build any application. Popularly known as a technology infrastructure or solutions stack, technology stack has become essential for building easy-to-maintain, scalable web applications.

Technology stack determines the type of applications you can build, the level of customizations you can perform, and the resources you need to develop your application.

For example, a web tech stack typically looks like:

![web stack](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-1/web%20stack.png)

There are acronymns for individual technologies used together for a specific technology product. some examples are…
* LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
* LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
* MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
* MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

And for this project, we are implemenitng LEMP stack solution.

---
## Connectint to the EC2 server through SSH
> NB: For this project, i already created an EC2 server on AWS, i just need to connect to it to start the implementation.


```ssh -i "mykey.pem" ubuntu@ec2-54-204-72-76.compute-1.amazonaws.com ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-2/ubuntu.JPG)

Then Update my software packages by running:

```sudo apt update ```

---

## INSTALLING THE NGINX WEB SERVER
NGINX is open-source web server software used for reverse proxy, load balancing, and caching. It provides HTTPS server capabilities and is mainly designed for maximum performance and stability. It also functions as a proxy server for email communications protocols, such as IMAP, POP3, and SMTP.

In order to display web pages to our site visitors, we are going to employ Nginx, a high-performance web server. We’ll use the apt package manager to install this package:

```sudo apt install nginx ```

To verify Nginx is running:

```sudo netstat -tulpn | grep nginx ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-2/Nginx.JPG)

---

## INSTALLING MYSQL

Now that we have a web server up and running, we need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database.
MySQL is a relational database management system (RDBMS) developed by Oracle that is based on structured query language (SQL).  MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

#### To install MYSQL using ubuntu's package manager we run the following commands

```sudo apt install mysql-server```

when prompted, confirm installation by typing y

connect to the database:

```sudo mysql```

It’s recommended that we run a security script that comes pre-installed with MySQL. This script will remove some insecure default settings and lock down access to our database system. Before running the script we will set a password for the root user, using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.2:

``` ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1'; ```

Then we Exit MYSQL shell:

``` exit ```

Then Start the interactive script by running:

``` sudo mysql_secure_installation ```

This asked if we'll like to set-up VALIDATE PASSWORD COMPONENT which we say NO

we can now log-in to MYSQL with the root password and run some queries:

``` sudo mysql -p ```

After successfully installing and testing mysql:

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-1/mysql.JPG)



