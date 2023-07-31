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

```sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates```

```curl -sL https://deb.nodesource.com/setup_18.x | sudo -E bash - ```


Install NodeJS

```sudo apt install -y nodejs```

## Installing MongoDB.
MongoDB stores data in flexible, JSON-like documents. Fields in a database can vary from document to document, and data structure can be changed over time. For our example application, we are adding book records to MongoDB that contain the book name, ISBN number, author, and number of pages.

To import the Public Key used by Package Management System:

```sudo apt-get install gnupg curl```

```curl -fsSL https://pgp.mongodb.com/server-6.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor``` 

To create a List file For MongoDB for Ubuntu 22.04:

```
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

```

Then we reload Local Packages

```sudo apt-get update```

Install MongoDB

```sudo apt-get install -y mongodb-org```

To start and enable the Mongod service:

```
sudo systemctl start mongod

sudo systemctl enable mongod

```
To verify that the service is up and running

```sudo systemctl status mongod```

### To Install the body-parser package.

We need the ‘body-parser’ package to help us process JSON files passed in requests to the server.

```sudo npm install body-parser```
Then we create a folder named Books and CD into the folder:

```mkdir Books && cd Books``

In the Books directory, we initialize npm project

```npm init```

And then we add a file named server.js

```touch server.js && vi server.js```

Then we copy and paste the web server code below into the server.js file.

```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();
app.use(express.static(__dirname + '/public'));
app.use(bodyParser.json());
require('./apps/routes')(app);
app.set('port', 3300);
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});

```

And then save and quit.

```:wq```

## Install Express and set up routes to the server.
Express is a minimal and flexible Node.js web application framework that provides features for web and mobile applications. We will use Express in to pass book information to and from our MongoDB database.

We also will use Mongoose package which provides a straightforward, schema-based solution to model our application data. We will use Mongoose to establish a schema for the database to store data of our book register.

```sudo npm install express mongoose```

In ‘Books’ folder, we create a folder named apps and CD into the folder

```mkdir apps && cd apps```

Then we create a file named routes.js

```touch routes.js && vi routes.js```

We then copy and paste the following codes;

```
const Book = require('./models/book');

module.exports = function(app){
  app.get('/book', function(req, res){
    Book.find({}).then(result => {
      res.json(result);
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while retrieving books');
    });
  });

  app.post('/book', function(req, res){
    const book = new Book({
      name: req.body.name,
      isbn: req.body.isbn,
      author: req.body.author,
      pages: req.body.pages
    });
    book.save().then(result => {
      res.json({
        message: "Successfully added book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while saving the book');
    });
  });

  app.delete("/book/:isbn", function(req, res){
    Book.findOneAndRemove(req.query).then(result => {
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    }).catch(err => {
      console.error(err);
      res.status(500).send('An error occurred while deleting the book');
    });
  });

  const path = require('path');
  app.get('*', function(req, res){
    res.sendFile(path.join(__dirname, 'public', 'index.html'));
  });
};

```

In the ‘apps’ folder, we create a folder named models:

```mkdir models && cd models```

We create a file named book.js

```vi book.js```

Then we copy and paste the following codes;

```
var mongoose = require('mongoose');
var dbHost = 'mongodb://localhost:27017/test';
mongoose.connect(dbHost);
mongoose.connection;
mongoose.set('debug', true);
var bookSchema = mongoose.Schema( {
  name: String,
  isbn: {type: String, index: true},
  author: String,
  pages: Number
});
var Book = mongoose.model('Book', bookSchema);
module.exports = mongoose.model('Book', bookSchema);

```
## Accessing the routes with AngularJS
AngularJS provides a web framework for creating dynamic views in our web applications. In this tutorial, we use AngularJS to connect our web page with Express and perform actions on our book register.

Change the directory back to ‘Books’

```cd ../..```

Then we create a folder named public and CD into the folder

```mkdir public && cd public```

We add a file named script.js

```vi script.js```

We copy and paste the Code below (controller configuration defined) into the script.js file.

```
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope, $http) {
  $http( {
    method: 'GET',
    url: '/book'
  }).then(function successCallback(response) {
    $scope.books = response.data;
  }, function errorCallback(response) {
    console.log('Error: ' + response);
  });
  $scope.del_book = function(book) {
    $http( {
      method: 'DELETE',
      url: '/book/:isbn',
      params: {'isbn': book.isbn}
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
  $scope.add_book = function() {
    var body = '{ "name": "' + $scope.Name + 
    '", "isbn": "' + $scope.Isbn +
    '", "author": "' + $scope.Author + 
    '", "pages": "' + $scope.Pages + '" }';
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response);
    }, function errorCallback(response) {
      console.log('Error: ' + response);
    });
  };
});

```

In the public folder, we create a file named index.html;

```vi index.html```

We then copy and paste the code below into the index.html file

```
<!doctype html>
<html ng-app="myApp" ng-controller="myCtrl">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
    <script src="script.js"></script>
  </head>
  <body>
    <div>
      <table>
        <tr>
          <td>Name:</td>
          <td><input type="text" ng-model="Name"></td>
        </tr>
        <tr>
          <td>Isbn:</td>
          <td><input type="text" ng-model="Isbn"></td>
        </tr>
        <tr>
          <td>Author:</td>
          <td><input type="text" ng-model="Author"></td>
        </tr>
        <tr>
          <td>Pages:</td>
          <td><input type="number" ng-model="Pages"></td>
        </tr>
      </table>
      <button ng-click="add_book()">Add</button>
    </div>
    <hr>
    <div>
      <table>
        <tr>
          <th>Name</th>
          <th>Isbn</th>
          <th>Author</th>
          <th>Pages</th>

        </tr>
        <tr ng-repeat="book in books">
          <td>{{book.name}}</td>
          <td>{{book.isbn}}</td>
          <td>{{book.author}}</td>
          <td>{{book.pages}}</td>

          <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
        </tr>
      </table>
    </div>
  </body>
</html>

```
Then we change the directory back up to Books

```cd ..```

We then start the server by running this command: 

```node server.js```

The server is now up and running, we can connect it via port 3300 or using our EC2 public ip address:3300.

![](https://github.com/Tolu4realluv/dareyio-pbl/blob/main/Project%204/Book-App.JPG)

This shows that our Book Register web form is ready and we can add as many as possible entries.










