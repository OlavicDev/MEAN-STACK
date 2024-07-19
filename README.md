# MEAN-STACK

The MEAN stack is a popular full-stack JavaScript framework that simplifies and accelerates web application development. The acronym "MEAN" stands for:

### MongoDB: A NoSQL database that stores data in a flexible, JSON-like format called BSON (Binary JSON). MongoDB is designed for scalability and ease of use, making it a good fit for modern web applications.

### Express.js: A lightweight and flexible Node.js web application framework. It provides a robust set of features for building web and mobile applications, including routing, middleware support, and more.

### Angular: A front-end framework developed by Google for building dynamic, single-page applications (SPAs). Angular uses TypeScript, a superset of JavaScript, and provides a rich set of tools and features for building complex user interfaces.

### Node.js: A runtime environment that allows you to execute JavaScript code on the server side. Node.js is built on the V8 JavaScript engine and is known for its non-blocking, event-driven architecture, which makes it ideal for building scalable and efficient web applications.

## Key Benefits of the MEAN Stack
1. Single Language: Since both the client-side and server-side code are written in JavaScript, development is streamlined, and there's no need to switch between different languages.

2. JSON Everywhere: Data flows seamlessly between all layers of the application in JSON format, from MongoDB to Angular, making data handling straightforward and efficient.

3. Full-Stack Development: The MEAN stack covers the entire web development process, from the database to the client interface, allowing developers to work on both the frontend and backend.

4. Scalability: Each component of the MEAN stack is designed with scalability in mind. MongoDB can handle large amounts of data, Node.js can manage numerous connections simultaneously, and Angular can build complex SPAs.

5. Open Source and Community Support: All components of the MEAN stack are open source, with large communities that contribute to their continuous improvement and offer support through forums, documentation, and tutorials.

## Typical Workflow in a MEAN Stack Application
1. Frontend (Angular): The user interacts with the application through the Angular frontend, which handles the user interface and user experience. Angular components communicate with the backend via HTTP requests.

2. Backend (Express.js and Node.js): The Express.js framework handles routing, middleware, and business logic on the server side. It processes incoming requests from the frontend, interacts with the database, and sends appropriate responses back to the client.

3. Database (MongoDB): MongoDB stores the application's data in a flexible and scalable manner. The backend communicates with MongoDB to perform CRUD (Create, Read, Update, Delete) operations.

By using the MEAN stack, developers can build robust, scalable, and maintainable web applications with a unified development environment.

# let a take short journey through it 
in this project we are deploying a mean stack application into an aws ubuntu server:

## Step1: install Nodejs :
### Update and Upgrade ubuntu
```
sudo apt update
sudo apt upgrate
```

### Add certifiates
```
sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
curl -sl https://deb.nodesource.com/setup_12.x | sudo -E bash -
```
### Install Node js:
```
sudo apt install -y node.js
```
![image](https://github.com/user-attachments/assets/12b06ca1-75af-4ec1-b6ba-a4d5e6fec014)

## Step2 Install MongoDB:
MongoDB stores in flexible, `json-like` documents. Fields in a databse can vary from document to document and data structure can be changed over time 

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A18585931BC711F9BA15703C6

echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntutrusty/mongodb-org/3.4 multiverse" | sudo tee
/etc/apt/sources.list.d/mongodb-org-3.4.list

```
### Install MongoDB:
```
sudo apt install -y mongodb-org
```
![image](https://github.com/user-attachments/assets/209f5b83-c028-40af-b72d-94c6aa637b6f)


### start the server
```
sudo service mongodb start
```
verify that the service is up and running :
```
sudo systemctl status mongodb
```
![image](https://github.com/user-attachments/assets/a55977c5-0246-47e9-843d-9ca64c13092a)


### install `npm` Node packege manager:
```
sudo apt install -y npm
```
### install body-parser package
```
sudo npm install body-parser
```
### create a folder named Books
```
mkdir Books && cd Books
```
![image](https://github.com/user-attachments/assets/81bf96bf-edf1-4cda-b14c-85270096913f)

in the Book dir initialize npm project
```
npm init
```
add a file to it named server.js
```
vim server.js
```
 copy and paste this:
 ```
var express = require('express');
var bodyParser = require('body-parser');
var app = express();

// Serve static files from the 'public' directory
app.use(express.static(__dirname + '/public'));

// Use bodyParser for parsing JSON request bodies
app.use(bodyParser.json());

// Import routes and pass the Express app instance
require('./apps/routes')(app);

// Set the port number
app.set('port', 3300);

// Start the server
app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
});

```
## Step3 Install Express and setup routes to the server
Express is a minimal and flexible `Node.js` web application framework that provides features for web and mobile applications

```
sudo npm install express mongoose
```
In Books folder create a folder named app 
```
mkdir apps && cd apps
```
Create a file `routes.js`
```
vim routes.js
```
Copy and paste
```
const Book = require('./models/book');
const path = require('path');

module.exports = function(app) {
  // Get all books
  app.get('/book', async (req, res) => {
    try {
      const books = await Book.find({});
      res.json(books);
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Add a new book
  app.post('/book', async (req, res) => {
    try {
      const book = new Book({
        name: req.body.name,
        isbn: req.body.isbn,
        author: req.body.author,
        pages: req.body.pages
      });
      const result = await book.save();
      res.json({
        message: "Successfully added book",
        book: result
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Update a book
  app.put('/book/:isbn', async (req, res) => {
    try {
      const updatedBook = await Book.findOneAndUpdate(
        { isbn: req.params.isbn },
        req.body,
        { new: true }
      );
      if (!updatedBook) {
        return res.status(404).json({ error: 'Book not found' });
      }
      res.json({
        message: "Successfully updated the book",
        book: updatedBook
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Delete a book
  app.delete('/book/:isbn', async (req, res) => {
    try {
      const result = await Book.findOneAndRemove({ isbn: req.params.isbn });
      if (!result) {
        return res.status(404).json({ error: 'Book not found' });
      }
      res.json({
        message: "Successfully deleted the book",
        book: result
      });
    } catch (err) {
      console.error(err);
      res.status(500).json({ error: 'Internal Server Error' });
    }
  });

  // Serve static files
  app.get('*', (req, res) => {
    res.sendFile(path.join(__dirname, '../public', 'index.html'));
  });
};

```
In the 'apps' folder, create a folder named models
```
mkdir models
cd models
```
Create a file `books.js`
```
vim book.js
```
copy and paste 
```
const mongoose = require('mongoose');

const bookSchema = new mongoose.Schema({
  name: { type: String, required: true },
  isbn: { type: String, required: true, unique: true },
  author: { type: String, required: true },
  pages: { type: Number, required: true }
});

module.exports = mongoose.model('Book', bookSchema);
```

## Step 4 Access the routes with AngularJS
AngularJS will be used to connect our web page with Express and perform actions on our book register Change directory to the books
```
cd ../..
```
Create and open a folder named public
```
mkdir public && cd public
```
Add and open a file named script.js
```
vim script.js
```
Copy and paste
```
var app = angular.module('myApp', []);

app.controller('myCtrl', function($scope, $http) {
  // Get all books
  function getAllBooks() {
    $http({
      method: 'GET',
      url: '/book'
    }).then(function successCallback(response) {
      $scope.books = response.data;
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  }

  // Initial load of books
  getAllBooks();

  // Add a new book
  $scope.add_book = function() {
    var body = {
      name: $scope.Name,
      isbn: $scope.Isbn,
      author: $scope.Author,
      pages: $scope.Pages
    };
    $http({
      method: 'POST',
      url: '/book',
      data: body
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
      // Clear the input fields
      $scope.Name = '';
      $scope.Isbn = '';
      $scope.Author = '';
      $scope.Pages = '';
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };

  // Update a book
  $scope.update_book = function(book) {
    var body = {
      name: book.name,
      isbn: book.isbn,
      author: book.author,
      pages: book.pages
    };
    $http({
      method: 'PUT',
      url: '/book/' + book.isbn,
      data: body
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };

  // Delete a book
  $scope.delete_book = function(isbn) {
    $http({
      method: 'DELETE',
      url: '/book/' + isbn
    }).then(function successCallback(response) {
      console.log(response.data);
      getAllBooks();  // Refresh the book list
    }, function errorCallback(response) {
      console.log('Error: ' + response.data);
    });
  };
});
```
In 'public' folder, create and open a file named index.html
```
vim index.html
```
Copy and paste the code below in it
```
<!DOCTYPE html>
<html ng-app="myApp" ng-controller="myCtrl">
<head>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
  <script src="script.js"></script>
  <style>
    /* Add your custom CSS styles here */
  </style>
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
    <div ng-if="successMessage">{{ successMessage }}</div>
    <div ng-if="errorMessage">{{ errorMessage }}</div>
  </div>
  <hr>
  <div>
    <table>
      <tr>
        <th>Name</th>
        <th>Isbn</th>
        <th>Author</th>
        <th>Page</th>
        <th>Action</th>
      </tr>
      <tr ng-repeat="book in books">
        <td>{{ book.name }}</td>
        <td>{{ book.isbn }}</td>
        <td>{{ book.author }}</td>
        <td>{{ book.pages }}</td>
        <td><button ng-click="del_book(book)">Delete</button></td>
      </tr>
    </table>
  </div>
</body>
</html>
```
Change the directory back to book
```
cd ..
```
Start the server by running this command
```
node server.js
```
![image](https://github.com/user-attachments/assets/2b24e80c-832c-4c08-8106-745e0c7f86fa)

check it out in a browser
![image](https://github.com/user-attachments/assets/19e2e695-bfd6-4229-b4d6-ebcf63c255ae)
our application is working right 

## Conclusion
The MEAN stack—comprising MongoDB, Express.js, Angular, and Node.js—provides a powerful and cohesive framework for building dynamic and scalable web applications. By leveraging JavaScript across both the client and server sides, developers can streamline the development process and enhance productivity.


 



