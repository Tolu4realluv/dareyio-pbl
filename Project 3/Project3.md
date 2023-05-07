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

To see our public ip address from the terminal:

```curl -s http://169.254.169.254/latest/meta-data/public-ipv4 ``` 

To see our public dns name from the terminal:

```curl -s http://169.254.169.254/latest/meta-data/public-hostname ```


![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/express.JPG)

We exit the node server shell by typing ctrl C.

### Routes

There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So we create a folder routes

```mkdir routes ```

We can now CD into the routes folder

Now, we create a file api.js with the command:

```touch api.js ```

And we open the file with vim:

```vim api.js ```

Then we copy and paste the code:

```
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;

```

### Models

Since the app is going to make use of Mongodb which is a NoSQL database, we need to create a model. A model is at the heart of JavaScript based applications, and it is what makes it interactive.

We will also use models to define the database schema . This is important so that we will be able to define the fields stored in each Mongodb document. In essence, the Schema is a blueprint of how the database will be constructed, including other data fields that may not be required to be stored in the database. These are known as virtual properties

To create a Schema and a model, we install mongoose which is a Node.js package that makes working with mongodb easier.

Change directory back Todo folder with cd Todo and install Mongoose:

```cd Todo ```

```npm install mongoose ```

We create a new folder models:

```mkdir models ```

And CD into models:

```cd models ```

Inside the models folder, we create a file and named it todo.js:

```touch todo.js ```

We can as well execute the three commands above on a single line with the help of &&:

```mkdir models && cd models && touch todo.js ```

We then open the file created with vim todo.js then paste the code below in the file:

```
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;

```

Now we need to update our routes from the file api.js in ‘routes’ directory to make use of the new model.

In Routes directory, open api.js with vim api.js, delete the code inside with :%d command and paste there code below into it then save and exit

```
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;

```

---

## MONGODB DATABASE

We need a database where we will store our data. MongoDB is a source-available cross-platform document-oriented database program. Classified as a NoSQL database program, MongoDB uses JSON-like documents with optional schemas. 

For this project, we will make use of mLab. mLab provides MongoDB database as a service solution (DBaaS), so to make life easy, we will will need to sign up for a shared clusters free account, which is ideal for our use case. [Sign up here](https://www.mongodb.com/atlas-signup-from-mlab). Follow the sign up process, select AWS as the cloud provider, and choose a region near us.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/mongo1.JPG)

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/mongo2.JPG)

Allow access to the MongoDB database from anywhere:

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/mongo3.JPG)


In the index.js file, we specified process.env to access environment variables, but we have not yet created this file. So we need to do that now.

We create a file in our Todo directory and name it .env:

```
touch .env
vi .env

```

And Add our connection string into our application code:

```mongodb+srv://Tolu4realluv:<password>@kaycuster.fgsh3gv.mongodb.net/?retryWrites=true&w=majority ```

Now we need to update the index.js to reflect the use of .env so that Node.js can connect to the database by deleting the existing content in the file, and update it with the entire code below:

```
const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});

```

Using environment variables to store information is considered more secure and best practice to separate configuration and secret data from the application, instead of writing connection strings directly inside the index.js application file.

Starting our server using the command:

```node index.js ```

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%203/todo%20server.JPG)

So far we have written backend part of our To-Do application, and configured a database, but we do not have a frontend UI yet. We need ReactJS code to achieve that. But during development, we will need a way to test our code using RESTfull API. Therefore, we will need to make use of some API development client to test our code.

In this project, we will use Postman to test our API. [Click](https://www.postman.com/downloads/) Install Postman to download and install postman on our machine.

Now we open our Postman, create a POST request to the API:

```http://52.206.208.144:5000/api/todos ```
This request sends a new task to our To-Do list so the application could store it in the database.






