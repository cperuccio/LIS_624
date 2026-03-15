## Installing and configuring MySQL (Week 09)
Date: March 15, 2026

### What is MySQL?
MySQL is a relational database. A relational database organizes data into tables (think metadata!).

### Installation and setup
1. To install: `sudo apt install mysql-server`
2. To confirm the version I'm using: `mysql --version`
3. Check the status: `systemctl status mysql`
	1. Can confirm that the mysql is `Loaded` and `Active`
4. To ensure a secure configuration of MySQL: `sudo mysql_secure_installation`
	1. Then you'll see prompts to respond to. As instructed, I answered **Y** (yes) for all—except password validation policy.
	2. I selected **LOW** for a weak password policy, but in real-world uses I might choose a stronger option.
5. Now you can log in: `sudo mysql -u root`
	1. This is the root user for special administrative functions.
6. You'll know you're encountering a MySQL prompt when you see `mysql>`
7. Not a bad idea to list available databases: `show databases;`
	1. Important: MySQL commands must end with semicolon
8. When I want to exit MySQL server prompt to return to Bash: `\q`

### Creating regular user account
1. From the MySQL server root, create a new user:
	1. ```create user 'opacuser'@'localhost' identified by 'password_goes_here';```
	2. If successful, prompt returns **Query OK** message

#### Creating a database
1. Run this command: ```create database opacdb default character set utf8mb4 collate utf8mb4_0900_ai_ci;```
2. View databases to see the new database: `show databases;`
3. Grant privileges for new database to my user: ```grant all privileges on opacdb.* to 'opacuser`@`localhost`;```
4. Exit: `\q`

### Logging in as my new user and creating tables
1. Make the MySQL command line less bare-bones: `nano ~/.bashrc`
2. Add provided: `export MYSQL_PS1=[\d]> "`
3. Save and exit: `source ~/.bashrc`
4. Log in to the new user I've created: `mysql -u opacuser -p`
5. Run the show databases command to see the new database: `show databases;`
4. Switch to the new database: `use opacdb;`
6. Run this:

> create table books (
	>>  id int unsigned not null auto_increment,
	>>  author varchar(150) not null,
	>>  title varchar(150) not null,
	>>  copyright year not null,
	>>  primary key (id)
>);

7. You've now created a new table called books. Open it in this way: `show tables;` and then `describe books;`

### Add records to the table
1. To add data, here's what you can run: `insert into books (author, title, copyright) values
2. On the next line, you can actually input the values that should go in each of the fields we've named. Here's an example: ```(`Jennifer Egan`, `The Candy House`, `2022`);```
3. View the reocrd I've created: `select * from books;`

###Install PHP and MySQL support
1. From bash, install PHP support for MySQL: `sudo apt install php-mysql`
2. Restart Apache and MySL: `sudo systemctl restart apache2` and `sudo systemctl` restart mysql`
3. For PHP to connect to MySQL, PHP needs to authenticate itself. 
	1. Change to my doc root's parent directory: `cd /var/www`
	2. Run the touch command to create a `login.php` file: `sudo touch login.php`
	3. Change file permissions: `sudo chmod 640 login.php`
	4. Change file  ownership: `sudo chown :www-data login.php`
	5. Next: `ls -l login.php`
	6. Open my new file: `sudo nano login.php`
4. In this new file, you'll add your credentials:
	<<?php //login.php
	<<$db_hostname = "localhost";
	<<$db_database = "opacdb";
	<<$db_username = "opacuser";
	<<$db_password = "password_is_here";
	<<?>
5. Create a new PHP file for the website: 
	1. Change to your apache root user: `cd /var/www/html`
	2. Create the new file: `sudo nano opac.php`
6. Added provided PHP text, save, and exit from nano.

### Testing syntax
1. `sudo php -f /var/www/login.php`
2. `sudo php -f /var/www/html/opac.php`

### Summary

1. MySQL organizes databases/data tables. PHP connects with MySQL to communicate this data to our Apache server.
2. To facilitate this connection, first we used MySQL to make a database (opacdb) containing our data table of information about books.
3. Then we created a PHP file (opac.php) in our Apache root directory (/var/www/html).
4. The PHP programming language in our opac.php file facilitates the connection with our MySQL opacdb database, and this is possible because the PHP authenticates itself by way of the login.php file we added to /var/www.
5. In this way, PHP retrieves the data from the MySQL database, processes the data as HTML, communicates the information in our opacdb database to the Apache server, and makes the data available on our browsers. 
6. We changed the permissions of the login.php file so that Apache can read it.
