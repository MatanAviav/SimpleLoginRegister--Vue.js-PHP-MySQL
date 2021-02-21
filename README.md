# SimpleLoginRegister - Vue.js Example With PHP & MySQL By Matan Aviav
This repository contains an example of a simple responsive login & register system based on Vue.js, PHP & MySQL database, built by me (Matan Aviav).

## Table of contents:
1. Introduction

2. Tools I used in this project

3. Originality

4. Security

5. Live demo

<br/><br/>
## 1. Introduction:
I wanted to build an example of a small login & register responsive system based on Vue.js, PHP & MySQL database. <br/>
The idea is simple: you register and then login. Note that this system is very simple and not including update of user details exept to image cover update.


### Screenshots of the system:
![GitHub Logo](https://i.ibb.co/LhXgBfS/4.png)

<br />

![GitHub Logo](https://i.ibb.co/8K2G3NN/3.png)

<br />

![GitHub Logo](https://i.ibb.co/fXvz8GR/1.png)

<br />

![GitHub Logo](https://i.ibb.co/SP458G2/2.png)

<br />



## 2. Tools I used in this project:


To write the code I used Visual Studio Code.
To run Vue.is I used Node.js server environment for Win64 and to run PHP (version 7.2) I used WampServer with built-in MySQL server.
In this project I used: Vue.js, Axios, JavaScript, Bootstrap 4, FontAwesome CSS, HTML5, PHP, SQL, HTACCESS.

* ***Vue.js:***<br/>
Vue.js is the dominant tool in this project. I built the whole project according it. Mainly, I used Vue.js to navigate between pages instead of reload pages in every request, and for using complex components. Of course, I exploited the main feature that Vue.js offers: asynchronic code. In order to send POST/GET requests, I decided to use 'Axios' module. In order to navigate between components, I used 'vue-router' module. I managed global state in Vue instance.

* ***Vanilla JavaScript:***<br/>
Vanilla JavaScript is the core of Vue.js. Without it, Vue.js would not exist. All Vue.js files are pure JS files.

* ***Bootstrap 4 & CSS:***<br/>
I used Bootstrap 4 in order to style the website.

* ***PHP & SQL:***<br/>
I used PHP (version 7.2) in order to handle all requests from client-side like: login & register. PHP is the core of the backend here. The idea is simple: when the user perform an action then Vue.js send a POST or GET request to the backend (PHP). When a specific PHP file get the request, it handles it and return the result back to React. According to the result returned, Vue.js performs a specific action. I used PHP to connect to MySQL server, and to write/get data to/from the database. I decided to use PDO PHP class to connect to MySQL server. SQL, of course, is used to prepare SQL statements in order to get data and to insert data. 

  MySQL tables structures:<br />
  ```
  CREATE TABLE IF NOT EXISTS `users` (
    `id` bigint(20) NOT NULL AUTO_INCREMENT,
    `name` varchar(40) NOT NULL,
    `pass` varchar(60) NOT NULL,
    `email` varchar(100) NOT NULL,
    `phone` varchar(100) NOT NULL,
    `img` varchar(100) NOT NULL,
    `cover` varchar(100) DEFAULT NULL,
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
  ```
  
* ***HTACCESS:***<br/>
I used HTACCESS to manage permissions to files and directories on the server. I used Options, Deny & Allow, FilesMatch and more features.


<br />


## 3. Originality:
All PHP files and JavaScript files have written by me (MatanAviav). I wanted to keep on originality during the whole project process.



<br />


## 4. Security:
In order to secure the system, I needed to take control on the data the user insert to the database, and also take control on the data that comes from the database. In addition, I had to find a way to make the user authentication method secure and to find a way to secure sensitive user data like passwords.

  1. *User Authentication:<br />*
  In order to make a secure user authentication, I decided to use a token-based authentication with JWT (JSON Web Tokens). In every successful login, the system take a specific action according to the user choice if to save the connection or not. If the user decided NOT to save the connection then all of his connections details will be saved in SERVER SESSION. If the user DID decided to save the connection then an unique JWT token will be created and will be saved in a httpOnly cookie. The decision to save the cookie as httpOnly is to prevent the access to document.cookie by hackers in case of XSS attack. When the user closes the browser and come back to the system, then the system automatically check the validity of the JWT token. Simple.
 
  2. *User Input (XSS and SQL Injection):<br />*
  In order to secure user input, I started with JavaScript validators (based on regular expressions) for any input. But that way is not enough, of course, because any user can edit the code and remove the validators because the code is on client-side. To solve that, I had to add my own validators to the backend. In PHP files that receive POST/GET data from any a React request, I took any input and checked whether its valid or not, using regular expressions and other techniques. That way, I took control of the user input but I it's still not enough. In order to fully secure user input when insert/get data to/from the database, I had to prevent SQL INJECTION. So, in order to prevent SQL INJECTION, I used prepared statements instead of regular queries. That way, I took full control of the user input. Of course, every problematic operation with the database, input rules, or working with files - is wrapped with try & catch blocks.
  
  3. *Data Comes From The Database (XSS):<br />*
  In order to prevent XSS, I had to take control of any type of script stored in the database that wish to be shown. I did not have to do almost anything here. The reason for that: by default, Vue.js does it automatically. See info here: https://vuejs.org/v2/guide/security.html
  
  4. *User Password:<br />*
  In order to secure user password in the database, I decided to use password_hash() PHP function, according to the recommendation in the official PHP documentation. I used this function in order to hash the password, with 2 SALT strings, and when the user try to login I used password_verify() PHP function to verify the password. The advantage of this technique is that password_hash() function does not create a CONSTANT hash - it can change in every function call.

  5. *Prevent CSRF Attacks:<br />*
  In order to prevent CSRF attakcs, I built a token-based mechanism to provide the user a token for any protected action he does in the website. The process is simple: the user gets a token from the backend, and when the user try to send a request from the client-side to the backend, the token will be sent automatically as an argument in the request. Then, when Vue.js gets the response, a new token fetched from the it (the response) and save it in Vue instance's data for the next request. That way, there is no possible way a hacker can manipulate a user to do any malicious action, because a hacker can't guess the token. That way, I can know for sure what is the source of the request.
  
  6. *Prevent Server Flooding:<br />*
  In order to prevent a lot of requests in very short time period (requests flooding), I built a mechanism to handle that. I wanted to block clients and logged users from sending too many requests to the backend. Note that the mose expensive operations are related to the database jobs. I had to save information about any client/user in order to recognize the client/user. I thought about saving their details in the database, but I wanted to prevent database flooding too. Finally, I decided to use files for that: for every suspicious client/user, I create a file and save into it an IP address or an user id number. In addition to that detail, I also save the time of the last request. In short: the mechanism works great.
  
  7. *Unrestricted File Upload:<br />*
  When a user register, he can add a  profile picture from his own device. In order to secure the process, I had to deal with many problems. The major problem is to identity if the picture the user want to upload is valid. To know that, I used PHP 'finfo' class, that check 'Magic Numbers'. Of course, I checked the suffix and the MIMETYPE of every file. In addition, at runtime I used chmod() function to give the only write/read permissions to every file, and with HTACCESS I force the response to be image/png or image/jpeg only. The access to .php files is forbidden.
  

<br />

## 5. Live demo:
To see a live demo of the system, please go to: http://matanvuejs.myartsonline.com/



