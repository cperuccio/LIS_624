# WordPress Installation
Date: Sunday, April 5, 2026

As a free and open source CMS, WordPress is a website builder. The public library where I previously worked used WordPress for its website!

## Installation instructions

1. Check [WordPress.org](https://wordpress.org/about/requirements/) to make sure that systems meet installation requirements. Knowing that WordPress requires PHP version 8.3 or greater and MySQL version 8.0 or greater, let's confirm ours:
	1. `php --version`: confirmed!
	2. `mysql --version`: confirmed!
2. Update our system before proceeding.
	1. Command I ran: `sudo apt update sudo apt upgrade -y sudo apt autoremove -y sudo apt clean`
3. Install PHP modules to support WordPress: `sudo apt install php-curl php-xml -php imagick php-mbstring php-zip php-intl`. Here's what they stand for:
	1. `php curl`: connecting and communicating with different types of servers 
	2. `php-xml`: reading and using XML data
	3. `php-imagick`: creating and modifying images
	4. `php-mbstring`: handling unicode (e.g., emojis)
	5. `php-zip`: reading or writing compressed files
	6. `php-intl`: performing "locale-aware operations" according to [PHP.net manual](https://www.php.net/manual/en/book.intl.php)
		1. As a note, this process took my VM a *long* time.
	
4. Restart Apache2 (`sudo systemctl restart apache2`) and restart mysql (`sudo systemctl restart mysql`)
5. Change the directory to the Apache root: `cd /var/www/html`
6. Download WordPress: `sudo wget https://wordpress.org/latest.zip`
	1. A note on downloading the latest version: on [this page](https://wordpress.org/download/) I hovered over the "Download WordPress 6.9.4 button" and looked at the URL at the bottom lefthand corner of the page ("overlink")
7. To unzip the files, I need install `sduo apt install unzip` 
8. Now I can unzip the files in the package: `sudo unzip latest.zip`
9. Doing this has resulted in a new directory called wordpress: `/var/www/html/wordpress`

## Creating a WordPress database and WordPress database user

We can create a WordPress database and database user in the same way we made a database and user for the bare-bones OPAC. Here's how:

1. Login in as the MySQL root user: `sudo mysql -u root`
2. Create the new user: `create user 'wordpress'@'localhost' identified by 'password_goes_here';`
	1. This means that our user is "wordpress"
3. Create the new database: `create database wordpress`
	1. That database is also "wordpress"
4. Grant privileges to the new user for the new database: `grant all privileges on wordpress.* to 'wordpress'@'localhost';`
5. Check that the database was successfully created: `show databases;`

## Connecting the new database to our site

You guessed it! We need to set up PHP to connect our new relational database in MySQL (wordpress) to our site.

1. From the wordpress directory, copy and rename `wp-config-sample.php` to `wp-config.php`:
	1. `sudo cp wp-config-sample.php wp-config.php`
2. We'll edit `wp-config.php` to add our own credentials: ` sudo nano wp-config.php`
	1. Update database name (wordpress), database user (wordpress), and password (the password I created when I was creating the user.
 
## Accessing the new install

1. Visit my IP address /wordpress: [my site](http://34.173.121.182/wordpress/)
2. I named my sate, entered my a new username and password (NOT the username and password I used to create the wordpress database).
3. Woohoo! I'm in WordPress! Time to set it up.
