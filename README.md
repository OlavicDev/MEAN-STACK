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


