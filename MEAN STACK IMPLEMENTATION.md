# Step 1: Install NodeJs

sudo apt update
Upgrade ubuntu
sudo apt upgrade

Add certificates
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -

Install NodeJS
sudo apt install -y nodejs

Step 2: Install MongoDB
we are adding book records to MongoDB that contain book name, isbn number, author, and number of pages.
images/WebConsole.gif
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
<img width="1371" alt="add data records from MongoDB" src="https://user-images.githubusercontent.com/97977311/150637680-71e9dd56-deef-42f0-92e2-704bde33a09e.png">
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

sudo apt install -y mongodb
sudo service mongodb start
sudo systemctl status mongodb

<img width="1068" alt="DynamoDB Status" src="https://user-images.githubusercontent.com/97977311/150637849-78be7746-a388-4a13-8464-a9ee9a9bff04.png">

Install npm – Node package manager.

Install npm – Node package manager.

sudo apt install -y npm
Install body-parser package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

sudo npm install body-parser
Create a folder named ‘Books’

mkdir Books && cd Books
In the Books directory, Initialize npm project

npm init
Add a file to it named server.js

vi server.js
Copy and paste the web server code below into the server.js file.

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

<img width="1068" alt="install npm package manager" src="https://user-images.githubusercontent.com/97977311/150637988-351c2b6f-78d9-4986-95f2-8335a24b20ff.png">

Install body-parser package

We need ‘body-parser’ package to help us process JSON files passed in requests to the server.

sudo npm install body-parser
Create a folder named ‘Books’

mkdir Books && cd Books
In the Books directory, Initialize npm project

npm init
Add a file to it named server.js

vi server.js
Copy and paste the web server code below into the server.js file.

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
sudo npm install express mongoose
In ‘Books’ folder, create a folder named apps

mkdir apps && cd apps
Create a file named routes.js

vi routes.js
Copy and paste the code below into routes.js

var Book = require('./models/book');
module.exports = function(app) {
  app.get('/book', function(req, res) {
    Book.find({}, function(err, result) {
      if ( err ) throw err;
      res.json(result);
    });
  }); 
  app.post('/book', function(req, res) {
    var book = new Book( {
      name:req.body.name,
      isbn:req.body.isbn,
      author:req.body.author,
      pages:req.body.pages
    });
    book.save(function(err, result) {
      if ( err ) throw err;
      res.json( {
        message:"Successfully added book",
        book:result
      });
    });
  });
  app.delete("/book/:isbn", function(req, res) {
    Book.findOneAndRemove(req.query, function(err, result) {
      if ( err ) throw err;
      res.json( {
        message: "Successfully deleted the book",
        book: result
      });
    });
  });
  var path = require('path');
  app.get('*', function(req, res) {
    res.sendfile(path.join(__dirname + '/public', 'index.html'));
  });
};
In the ‘apps’ folder, create a folder named models

mkdir models && cd models
Create a file named book.js

vi book.js
Copy and paste the code below into ‘book.js’

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
Step 4 – Access the routes with AngularJS
AngularJS to connect our web page with Express and perform actions on our book register.

Change the directory back to ‘Books’

cd ../..
Create a folder named public

mkdir public && cd public
Add a file named script.js

vim script.js
Copy and paste the Code below (controller configuration defined) into the script.js file.

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
In public folder, create a file named index.html;

vim index.html
Cpoy and paste the code below into index.html file.

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
Change the directory back up to Books

cd ..
Start the server by running this command:

node server.js

open port 3300 in EC2 SG

<img width="1360" alt="SG Port 3300" src="https://user-images.githubusercontent.com/97977311/150639173-ba2bf3b3-7f0b-463c-884d-93a97e180652.png">
http://18.215.182.116:3300/
<img width="1134" alt="Books Register" src="https://user-images.githubusercontent.com/97977311/150639568-ce4669f8-c802-4ee8-af8c-73943193d467.png">
<img width="1283" alt="Backend Book record" src="https://user-images.githubusercontent.com/97977311/150639559-d81f656c-5509-4c6d-8571-ed83eaca2915.png">
