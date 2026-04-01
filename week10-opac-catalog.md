# Creating a Barebones OPAC and Cataloging Module (Week 10)
Date: March 31, 2026

## Relational Databases
Relational databases manage and retrieve data efficiently by way of spreading and connecting data across multiple tables.

### Creating a new database

1. Log into MySQL root user: `sudo mysql -u root`
2. Create a new database: `mysql> create database Name_of_database;`
3. Grant privileges to the opacuser: `mysql> grant all privileges on Name_of_database.* to 'opacuser'@localhost';` 
4. Exit root user account: `mysql> \q`

### Create tables in a relational database

1. Log in as the `opacuser`: `mysql -u opacuser -p` and enter password accordingly.
2. Check that the new database was added: `show databases;`
3. If it's there, change to the new database: `use Name_of_database;`
4. Create a table with values: use `create table Name_of_table (`
5. The subsequent lines will include values (and be sure to start with `name__id auto_increment primary key` for an integer to be assigned as a unique identifier)
6. Add data to tables based on values created in step 5. Here is an example from this week's textbook:
```
insert into Meals (meal_name, cuisine, cooking_time, vegetarian) values
    ('Spaghetti Bolognese', 'Italian', 45, FALSE),
    ('Vegetable Stir Fry', 'Chinese', 20, TRUE),
    ('Chicken Curry', 'Indian', 50, FALSE),
    ('Mushroom Risotto', 'Italian', 35, TRUE);
```
7. Query data: `select * from Name_of_table;` returns all contents of the table. You can also search by value. Here's an example: `select * from Meals where vegetarian = TRUE;` filters results by the "vegetarian" value in the table. 
8. Use the `join` command to cross-reference tables based on a shared value. 

#### Adding tables to opacdb

If I want to add more records to by **books** table in my opacdb database, here's how:
1. Again, log in as `opacuser` (`mysql -u opacuser -p`) and select the `opacdb` database: `use opacdb`
2. Add new records with the `insert command`:
```
insert into books
(author, title, publisher, copyright) values
```
	1. Note: copyright value is listed as YYY-MM-DD 

### Database management

1. If you want to review the privileges granted to `opacuser` (or any user, really), run this command from the root user:`mysql> show grants for 'opacuser'@'localhost';`
2. You can revoke user privileges from specific databases: `mysql> revoke all privileges on Name_of_database.* from 'opacuser'@'localhost';`
3. To delete a database, use the drop command: `mysql> drop database Name_of_database;`
4. You can also delete a user account: `mysql> drop user 'name_of_user'@'localhost';`

## Creating a Bare Bones OPAC
An ILS is essentially built on dozens of interconnecting tables. The OPAC we create this week allows user to retrieve the data we entered into our database (opacdb). Here's how.

1. Change the directory to the Apache document root: `cd /var/www/html/`
2. Create an html page in the text editor: `sudo nano mylibrary.html`. Based on the provided code, this HTML page contains a form for searching the OPAC. When someone submits the button on the form, the form activates `search.php`. This PHP script connects to our relational database (opacdb) and contains a query to search the **books** table specifically.
	1. Note this line of the code in `mylibrary.html`:  `<form method="post" action="search.php">`
3. We must create the PHP search script from the Apache root directory: `sudo nano search.php`. At this point, I added the provided code.
4. Remember that PHP needs to authenticate itself in order to connect to MySQL. Sure enough, this PHP script connects to MySQL: 
```
<?php
// Load MySQL credentials
require_once 'var/www/login.php`;
```
	1. We created `login.php` file last week (see week09-mysql.md in this repository)

## Creating a Bare Bones Cataloging Module
While we've added records to our opacdb using the MYSQL commandline, this bare-bones catalogiing module will provide a graphical interface for adding records.

1. As before, we need to create an HTML page and PHP page in order to interact with our MySQL relational database.
2. To do this, we'll create a new directory: `/var/www/html/cataloging`
```cd /var/www/html
sudo mkdir cataloging
```
3. In this new directory, we'll create an HTML file with code to create a form for users to enter data:
```cd cataloging
sudo nano index.html
```
	1. In the file, note that the form references a PHP file that we will create:
`<form action="insert.php" method="post">`
	2.  Also note that the HTML form matches the data structure of our books table (e.g., book title, publisher, copyright, etc.)
5. Indeed, we need to create the PHP file! The PHP file will allow us communicate information from our form (`index.html`) to our relational database!
	1. `sudo nano insert.php` from the cataloging directory
	2. Add provided script
	3. Notice the provided authentication script!

### Security considerations
The HTML and PHP files we just created allow us to enter data into opacdb from a web interface, so we can implement a password mechanism that Apache2 server provides: htpasswd. In a real-life setting, this wouldn't be secure enough.
1. Change directory: `/etc/apache2`
2. Run this command: `sudo htpasswd -c /etc/apache2/.htpasswd libcat`
	1. In this example, `libcat` is the username
	2. Enter password accordingly (hashed)
	3. We used the -c option above because this was the first time we created the file. Don't use the -c option if I want to add other users.
3. Run this command to tell Apache2 that htpasswd will control access to the cataloging module: `sudo nano /etc/apache2/apache2.conf
4. In the apache2.conf file, add the following code:
```
<Directory /var/www/html/cataloging/>
  Options Indexes FollowSymLinks
  AllowOverride AuthConfig
  Require all granted
</Directory>
```
	1. Notice that this names the new directory we made earlier `/var/www/html/cataloging`
5. Change to the cataloging directory and create a file: `sudo nano .htaccess`
6. Add this:
```
AuthType Basic
AuthName "Authorization Required"
AuthUserFile /etc/apache2/.htpasswd
Require valid-user
```
	1. Note what's happening here: We're specifying that authorization is required (via htpasswd, specifically!) to request access for the information in our new cataloging directory.

