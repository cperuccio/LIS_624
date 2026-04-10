# Omeka Installation
Date: Friday, April 10, 2026

Omkea is an open-source platform for creating digital collections. Here's how I installed it.

## Installation instructions & troubleshooting

### Preparation

1. I updated my system: `sudo apt update` and `sudo apt upgrade`.
2. I confirmed that both PHP and MySQL met Omeka's installation requirements by running `php --version` and `mysql --version`.
3. I installed ImageMagick (`sudo apt install imagemagick`) to support photo files.
4. I enabled Apache `mod_rewrite` for creating user-friendly URLs (`sudo a2enmod rewrite`) and restarted Apache accordingly (`sudo systemctl restart apache2`).
	1. This step would become important later on for troubleshooting!
5. I logged in as the MySQL root user (`sudo mysql -u root`), and then  I  created a new user and a new database in MySQL for the Omeka installation:
	1. Creating username: create user `'omeka'@'localhost' identified by 'password;`
	2. Creating database: `create database omeka;`
	3. Granting privileges to the new user for the new database: `grant all privileges on omeka.* to 'omeka'@'localhost';`
	4. Confirming that I see my new database: `show databases;`

### Downloading and installation

1. I changed to my Apache root directory: ` cd /var/www/html/`
2. I ran `sudo wget (https://github.com/omeka/Omeka/releases/download/v3.2/omeka-3.2.zip)` to download the latest Omeka Classic release.
3. I ran `sudo unzip omeka-3.2.zip`.
4. As a result of these steps, I've created a new directory: `/var/www/html/omeka-3.2`
	1. I changed the name of the directory: `sudo mv /var/www/html/omeka-3.2 /var/www/html/omeka`
		1. I couldn't remember this command, but luckily I remembered that Dr. Burns covered this during WordPress installation!
	2. Now I have a new directory: `/var/www/html/omeka`
5. From my new directory, I ran the `ls` command to find the `db.ini` file.
	1. I ran `sudo nano db.ini` to see the contents of this file.
6. I added appropriate information to this file:
	1. host     = "localhost"
	2. username = "omeka"
	3. password = "password"
	4. dbname   = "omeka"
	5. I didn't change the final three (prefix, charset, ;port)
7. I used the `chown` command to give Apache file access (`sudo chmod -R g+w*`), and I restarted Apache.
	1. I didn't run the `chmod` at this time, which might have caused me some problems later~
8. The moment of truth: I went to [my site](http://34.173.121.182/omeka/), and... I saw an error message:
	1. "Omeka Installation Error. Mod_rewrite is not enabled. Apache’s mod_rewrite extension must be enabled for Omeka to work properly. Please enable mod_rewrite and try again." The troubleshooting begins!

### Troubleshooting

Before I document how I troubleshot this issue, I'll note that there something pretty exciting about the troubleshooting process. I know that there *has* to be an answer, and I just need to figure it out! I have more of a humanities background where so much of my tasks are interpretive. There was something really exciting about running into an issue, trying a few options, and finally seeing the success of my efforts!

1. The error message confused me because I *already* enabled Mod_rewrite. I figured I'd try running it again (`sudo a2enmod rewrite`). As excpected, I received a message that said "Rewrite already enabled." I restarted Apache again and visited the website, but still no luck.
2. This is when I realized I didn't run the `chown` command (`sudo chown :www-data /var/www/html`). I remembered this command from our installation of the cataloging module.
3. I ran the `chmod` command again and restarted Apache again, thinking this would fix it! 
	1. Alas, no luck! 
	2. Time to consult the internet...
4. I googled the error message I received when I visited my Omeka site, and I found [this post](https://forum.omeka.org/t/mod-rewrite-is-not-enabled/12468) on an Omeka forum. It's a few years old, but someone from the Omeka team (jflatnes) responded with actual steps to try.
5. Here's one of the reasons the person from the Omeka team said this problem might be happening: "The .htaccess file is not in place in the top level of the Omeka installation. This file contains the necessary rewrite rules and if it’s missing you’ll get this error. It’s in the correct place when unzipped, but since it’s a hidden file, it’s possible to accidentally “lose” it if you’re moving files around, so check that it’s there."
	1. Ok! Worth a shot: From the Omeka forum, I found a [post](https://forum.omeka.org/t/cannot-find-htaccess-to-enable-logs/9325) with instructions on finding the `.htaccess` file
	2. I found the file in my Omeka directory by running `ls -a`, and then I opened the file with nano. So the file was in the right place. In this file, though, I saw an interesting note: "If you know mod_rewrite is enabled, but you are still getting mod_rewrite errors, uncomment the line below and replace "/" with your base directory."
		1. Promising! I added `/omeka` after "RewriteBase," saved it, restarted Apache again, and visited my website...
		2. No luck! I wondered if I should have removed the backslash to rewrite my file as "RewriteBase omeka." So I tried this... and still, no go.
6. Ok. Let's try something else. Jflatnes from the Omeka forum suggested that Apache may not be configured to read .htaccess files. 	 
	1. To do this, I needed to figure out how to find this setting. I found the answer [here](https://stackoverflow.com/questions/18740419/how-to-set-allowoverride-all).
		1. I changed my directory: `cd /etc/apache2`.
			1. It made sense to me that I needed to leave my Omeka directory, given that the forums pointed to an *Apache* issue, rather than an Omeka issue.
		2. I ran `ls` to see my files.
		3. From the foru, I knew I needed to edit the `apache2.conf` file: `sudo nano apache2.conf`.
		4. Aha! I saw under `<Directory /var/www/>` this text: `AllowOverride None`. I changed it to `AllowOverride All`.
		5. I tried [my website](http://34.173.121.182/omeka/) again...
			1. Success!

## Reflection
This was the most troubleshooting I’ve done in the course so far. I acknowledge that I would have never solved this without advice from other people online, but now I think that I could help someone if they ran into these issues. 

On another note, I haven’t had the time to sort through what the issue is with adding images to my WordPress site. I will definitely get there—this week has proven that time, perseverance, and the internet can help me figure things out—but I’ll likely get there on Sunday evening (Sunday, April 12). (I have to travel to a wedding in a couple hours!) Stay tuned!





