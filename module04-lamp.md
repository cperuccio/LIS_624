## Module 4: Server setup documentation
Date: March 15, 2026

### What's the  LAMP stack?
The LAMP stack represents a group of open-source software (**L**inux, **A**pache, **M**ySQL, and **P**HP) commonly used together to host websites and applications with databases.

Here's how the components of the LAMP stack function together.
1. **Linux**: Linux is the operating system, and users interact with operating systems through an interface (e.g., graphical user interface or command line interface). 
2. **Apache**: Apache is the web server. Web servers store files, and because files can also store code, then web servers can run code, too. When a web browser requests access to retrieve or display a web server's files, the server can provide access (or indicate the error preventing access).
3. **MySQL**: MySQL is a relational databases that stores and organizes databases and data tables.
4. **PHP**: PHP is a server-side programming language that can retrieve data from MySQL and process the results as HTML—which the web server can process.

The LAMP stack is commonly used because the four components are open-source and free to use. Because of this accessibility and popularity, web users and developers may likely find troubleshooting support from the broader IT and systems librarianship community.  

### Installation, configuration, and verification

#### Apache
1. To install Apache, run this command: `sudo apt install apache2`.
	1. `apache2` is the name of the package to install, but run `apt show apache2` if you want to learn more information about the package before installation.
2. Run the `systemctl` command to get the status of `apache2` once installed.
	1. Once you run `systemctl status apache2`, look to see that the package is **enabled** and **active**.
	2. If the package is **enabled**, then it will start automatically once you reboot the operating system (by way of running `systemctl`.
	3. If the package is **active**, `apache2` is actually running.

#### PHP
1. To install both PHP and a package that creates the connection between PHP and Apache, run the `apt install` command: `sudo apt install php libapache2-mod-php`.
	1. Note that you can install multiple packages at the same time with the `apt install` command, as seen above.
2. Restart Apache (`sudo systemsctl restart apache2`) and then check the status to make sure Apache is still active and enabled (`systemctl status apache2`).
3. If Apache is running, then check that PHP has been successfully installed to work with Apache. To do this, change to Apache's document root directory: `cd /var/www/html/`
4. Then create a `.php` file: `sudo nano info.php`
	1. Add text to the file: 
```
<?php	
phpinfo();
?>
```
5. If PHP is working with Apache as it should, test your public IP address with `info.php` added to the end of the IP address url.
	1. Delete that file after the test (security reasons): `sudo rm /var/www/html/info.php`
6. The URL of the IP address will default to the file, `index.html`. But if you want to provide PHP to Apache, then you want Apache to display `index.php` as the default. Here's how:
	1. Edit the `dir.conf` file in the `/etc/apache2/mods-available/` directory. That file contains a line starting with `DirectoryIndex`, and Apache displays files in the order of appearance in this index.
	2. To do this, change the directory: `cd /etc/apache2/mods-available/`
	3. Make a backup copy of the original `dir.conf` file: `sudo cp dir.conf dir.conf.bak`
	4. Edit the original: `sudo nano dir.conf`
	5. On the `DirectoryIndex` line, change the order of files so that `index.php` precedes `index.html`.
	6. General practice: run `apachectl` to check the status of any configuration change: `apachectl configtest`
	7. Look for `Syntax Ok`, and then reload Apache and check the status.
		1. `sudo systemctl reload apache2`
		2. `systemctl status apache2`: **enabled** and **active** is the goal!

#### MySQL
1. To install MySQL, run the apt install command: `sudo apt install mysql-server`.
2. Check that it's running and enabled: `systemctl status mysql`. We're looking for **loaded** and **active**.
3. Next, install `mysql_secure_installation` for a secure configuration of MySQL: `sudo mysql_secure_installation`. Respond to all prompts that follow as appropriate.
4. Create a regular user account in MySQL:
	1. ```create user 'opacuser'@'localhost' identified by 'XXXXXXXXX';```
5. In the regular user account, I create a database (`create database opacdb`) and add a table to the database. (In this example, the new database is `opacdb`. From there, add records to the table.
6. To support the connection between PHP and MySQL, install another module: `sudo apt install php-mysql`
7. Restart both Apache and MySQL accordingly:
	1. `sudo systemctl restart apache2`
	2. `sudo systemctl restart mysql`
8. PHP also needs to authenticate itself in order to connect to MySQL databases. To do so, add a `login.php` file to the document root's parent directory:
	1. Change to parent directory of document root: `cd /var/www`
	2. Create the file: `sudo touch login.php`
	3. Change file permissions: `sudo chmod 640 login.php`
	4. Change file ownership: `sudo chown :www-data login.php`
	5. Add credentials to the `login.php` using `sudo nano login.php`. Here it is line by line:
		1. `<php // login.php`
		2. `$db_hostname = "localhost";`: remember, the hostname is the name of the regular user account you created in step 4.
		3. `$db_database = "opacdb";`: `opacdb` is the database you created in step 5.
		4. `$db_username = "opacuser";`: see step 4.
		5. `$db_password = "XXXXXXXXX";`: note the password created during step 4.
		6. `?>`
		7. As full code
```
<?php // login.php
$db_hostname = "localhost";
$db_database = "opacdb";
$db_username = "opacuser";
$db_password = "XXXXXXXXX";
?>
```
9. Next create a PHP file in the Apache root. 
	1. First change to the apache root: `cd /var/www/html`.
	2. Create a file in this root that interact with the database you created in step 5.
		1. For this exercise:`sudo nano opac.php`.
		2. PHP text added to this file which will support PHP authentication in the MySQL database. 
10. Final step: test the PHP syntax! If there are errors, a line number will be indicated:
	1. `sudo php -f /var/www/login.php`
	2. `sudo php -f /var/www/html/opac.php`
	3. View the site on a browser using the public IP address.

### Challenges

When I was preparing PHP to authentiate itself to connect to the opacdb database in MySQL, I ran into trouble in step number 10 (above). When I ran the `sudo php -f /var/www/login.php` command, I did see an output—which indicated an error. Even though the line number of the problem was indicated, it took me a few minutes to locate the issue: I had added an extra `<` in line 5.1 (above). Rather than a first line that reads `<php // login.php`, my erroneous line was written like this: `<php < // login.php`. It goes to show that attention to detail is important for installing the LAMP stack! 
