# WEB STACK IMPLEMENTATION (MERN STACK) IN AWS
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

And for this project, we are implemenitng MERN stack solution.

MERN Web stack consists of following components:

1. MongoDB: A document-based, No-SQL database used to store application data in a form of documents.
2. ExpressJS: A server side Web Application framework for Node.js.
3. ReactJS: A frontend framework developed by Facebook. It is based on JavaScript, used to build User Interface (UI) components.
4. Node.js: A JavaScript runtime environment. It is used to run JavaScript on a machine rather than in a browser.

For this project, we are creating a simple to-do application on mern web stack

---
## Connecting to the EC2 server through SSH
> NB: For this project, i already created an EC2 server on AWS, i just need to connect to it to start the implementation.


```ssh -i "mykey.pem" ubuntu@ec2-52-206-208-144.compute-1.amazonaws.com ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/ubuntu.JPG)

Then Update my software packages by running:

```sudo apt update ```

---

Then upgrade ubuntu by running:

```sudo apt upgrade ```

## BACKEND CONFIGURATION

Firstly, Lets get the location of Node.js software from Ubuntu repositories.

```curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - ```

Then we installed Node.js and npm with the command:

``` sudo apt-get install -y nodejs ```

We verify the node installation with the command:

```node -v ```

``` npm -v ``` 

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/Node.JPG)

### Setting up our Application code

First we create a new directory for our To-Do project:

```mkdir Todo ```

We check for the folder by using the 'ls' command:

```ls ```

And then we CD into our new diectory:

```cd Todo ```

Next, we used the command npm init to initialise our project, so that a new file named package.json will be created. This file will normally contain information about our application and the dependencies that it needs to run. We followed the prompts after running the command by pressing Enter several times to accept default values, then accept to write out the package.json file by typing yes.

```npm init ```

---

## INSTALLING EXPRESSJS

Express is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications. Express is a framework for Node.js, therefore a lot of things developers would have programmed is already taken care of out of the box. Therefore it simplifies development, and abstracts a lot of low level details. For example, Express helps to define routes of our application based on HTTP methods and URLs.

To use express, install it using npm:

```npm install express ```

Now we create a file index.js with the command:

```touch index.js ```

Then we install the dotenv module:

DotEnv is a lightweight npm package that automatically loads environment variables from a .env file into the process. env object. To use DotEnv, we first install it using the command: npm i dotenv . Then in our app, require and configure the package like this: require('dotenv').

```npm install dotenv ``` 

We open the index.js file with the command:

```vim index.js ``

We then type the following code, save and quit vim:

```
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
}); 

```

Now it is time to start our server to see if it works by opening the terminal in the same directory as our index.js file and type:

```node index.js ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/index.JPG)

We can now access our App by updating our EC2 security group to allow inboud connection on port 5000 as listed in our code.





