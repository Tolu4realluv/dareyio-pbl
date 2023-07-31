# WEB STACK IMPLEMENTATION (MERN STACK) IN AWS
---

## What is a Technology stack?
---
A stack is an arrangement of “things” kept in order one over the other. A technology stack is a set of technologies stacked together to build any application. Popularly known as a technology infrastructure or solutions stack, technology stack has become essential for building easy-to-maintain, scalable web applications.

The technology stack determines the type of applications you can build, the level of customizations you can perform, and the resources you need to develop your application.

For example, a web tech stack typically looks like this:

![web stack](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project-1/web%20stack.png)

There are acronyms for individual technologies used together for a specific technology product. Some examples are…
* LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
* LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
* MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
* MEAN (MongoDB, ExpressJS, AngularJS, NodeJS

And for this project, we are implementing a MEAN stack solution.

MEAN Web stack consists of the following components:

1. MongoDB: A document-based, No-SQL database used to store application data as documents.
2. ExpressJS: A server-side Web Application framework for Node.js. Makes requests to Database for Reads and Writes
3. Angular: (Front-end application framework) – Handles Client and Server Requests
4. Node.js: A JavaScript runtime environment. It runs JavaScript on a machine rather than in a browser. It accepts requests and displays results to the end user.

In this Project, we are implementing a simple Book Register web form using MEAN stack.

---
## Connecting to the EC2 server through SSH
> NB: For this project, I already created an EC2 server on AWS. I need to connect to it to start the implementation.

```ssh -i "mykey.pem" ubuntu@ec2-3-89-215-83.compute-1.amazonaws.com```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%204/ubuntu%20connect.JPG)

## Installing NodeJs

Node.js is a JavaScript runtime built on Chrome’s V8 JavaScript engine. Node.js is used in this project to set up the Express routes and AngularJS controllers.

Firstly, I Update my software packages by running:

```sudo apt update ```

Then upgrade Ubuntu by running:

```sudo apt upgrade -y```

Add certificates

``` sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates ```

```curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash - ```


