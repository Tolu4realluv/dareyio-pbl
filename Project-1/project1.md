# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS
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

And for this project, we are implemenitng LAMP stack solution.

---
## Connectint to the EC2 server through SSH
> NB: For this project, i already created an EC2 server on AWS, i just need to connect to it to start the implementation.
 
```ssh -i "mykey.pem" ec2-user@ec2-54-152-255-29.compute-1.amazonaws.com ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-1/Connect.JPG)

---
## INSTALLING APACHE AND UPDATING THE FIREWALL
### What is Apache?
Apache HTTP Server is the most widely used web server software. Developed and maintained by Apache Software Foundation, Apache is an open source software available for free. It runs on 67% of all webservers in the world. It is fast, reliable, and secure. It can be highly customized to meet the needs of many different environments by using extensions and modules. Most WordPress hosting providers use Apache as their web server software. However, websites and other applications can run on other web server software as well. Such as Nginx, Microsoft’s IIS, etc.

The Apache web server is among the most popular web servers in the world. It’s well documented, has an active community of users, and has been in wide use for much of the history of the web, which makes it a great default choice for hosting a website.

#### To install Apache using ubuntu's package manager we run the following commands
``` sudo apt update  ```

``` sudo apt install apache2  ```

> NB: if your operating system is Amazon Linux based AMI, use yum package manager instead of apt
> you can follow the instruction on this [Link](https://docs.rackspace.com/support/how-to/centos-7-apache-and-php-install/) to install Apache on AMI 


After successfull installation, it takes us to the test page for Apache 2


![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-1/Apache.JPG)
---

## INSTALLING INSTALLING MYSQL

Now that we have a web server up and running, we need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database.
MySQL is a relational database management system (RDBMS) developed by Oracle that is based on structured query language (SQL).  MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

#### To install MYSQL using ubuntu's package manager we run the following commands

```sudo apt install mysql-server```

when prompted, confirm installation by typing y

After successfully installing mysql

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-1/mysql.JPG)

---

## INSTALLING PHP

We have Apache installed to serve your content and MySQL installed to store and manage our data. PHP is the component of our setup that will process code to display dynamic content to the end user. In addition to the php package, we’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. we’ll also need libapache2-mod-php to enable Apache to handle PHP files. Core PHP packages will automatically be installed as dependencies.

#### To install these 3 packages at once, run:

```sudo apt install php libapache2-mod-php php-mysql```


