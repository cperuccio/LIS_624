## Installing and configuring PHP (Week 8)
Date: March 4, 2026

### What is PHP?
PHP is a server-side programming language. It needs to be installed on servers because browsers don't process PHP directly—the server does. Then the server sends the content to the browser.

### Installation 

1. To install: `sudo apt install php libapache2-mod-php`
2. Then restart Apache: `sudo systemctl restart apache2`
3. Confirm installation with this command: `php -v`
4. Check the status: `systemctl status apache2`

Following these steps, I can confirm that PHP is active (running).

### To check installation

Is PHP working with Apache, our server?

1. Change to the document root: `cd /var/www/html/`
2. At this point I had files: index.html and index.original.html (my backup of the original text).
3. We'll create a PHP file in the document root, and I'm doing this is nano: `sudo nano info.php`
4. I used the code that Dr. Burns provided.
5. Success when I tested in my browser with my IP address/info.php

### A note on Apache
Apache is configured to serve a web page titled index.html (even if that file isn't displayed in the URL bar!). That's why, last week, adding `index.html` to my IP address displayed the same page as the IP address alone.

### Configuration

To configure my IP address to go directly to `index.php` rather than `index.html`, here's what I'll do:

1. Change my directory: `cd /etc/apache2/mods-available/`
2. When I run `ls` I looked for `dir.conf`
3. Remember, I'm not in my home directory anymore, so I've got to use sudo. I'm running this command now to copy the `dir.conf` file: `sudo cp dir.conf dir.conf.bak`
4. Run `ls` to make sure I made the copy.
5. Open my file in nano: `sudo nano dir.conf`
6. Now I'll see a list of files, and the files are listed in order of appearance. (The first one listed is `index.html` which is why my IP address brings me directly there.
7. Move `index.php` to the start of the list, and save the file. Now this file should appear first.
8. After we configure something, run this command to check configuration: `apachectl configtest`. (Should say Syntax OK.)
9. If we see the syntax OK message, reload Apache: `sudo systemctl reload apache2`
10. Is it up and running? Try `systemctl status apache2`

### Creating our PHP page

1. Return to the root directory: `cd /var/www/html/
2. Make a new file (that will now appear first!): `sudo nano index.php`
3. I used the code that Dr. Burns provided.
4. Save and exit.
5. Confirm that the public IP address now directs to the `index.php file`

Success!
