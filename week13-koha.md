# Koha Installation
Date: Tuesday, April 21, 2026

Koha is a free, open-source ILS used around the world, mostly at small or medium libraries. Here's how I installed it.

## Installation instructions

1. I created a new VM through google cloud.
	1. Set machine configuration to e2-medium (2 vCPU, 1 core, 4 GB)
	2. Selected Ubuntu 24.04 LTS, 20GB Hard Disk for OS and storage
	3. Under networking, selected Allow HTTP traffic
	4. Created two network tags:
		1. koha-staff-8080 (staff interface)
		2. koha-opac-8081 (public interface/public opac)
	5. Created two firewall rules to open internet traffic to two ports
		1. Under VPC network and then Firewall, selected “Create a firewall rule” and added koha-staff-8080. 
		2. Created a second firewall: koha-opac 8081
		3. These ports matched the network tags I created in step 1v. 
		4. Source IPv4 ranges set to 0.0.0.0/0 to allow access from anywhere/any IP address.
2. I prepared my VM for installation.
	1. I updated my system.
	2. I ran this command ((`sudo apt autoremove -y && sudo apt clean`) to remove packages I no longer need and to clean out retrieved package files that I don’t need.)
	3. I ran the following commands:
		1. `sudo apt install apt-transport-https ca- certificates curl`
		2. `sudo mkdir -p –mode=775 /etc/apt/keyrings`
		3. `sudo curl -fsSL https:/debian.koha-community.org/koha/gpg.asc -o /etc/apt/keyrings/koha.asc`
			1. These commands allow me to set up “signing keys” to make sure I download authentic software.
	4. I accessed the root Linux user through the `sudo su` command.
		1. I pasted this command in my terminal:
``` tee /etc/apt/sources.list.d/koha.sources <<EOF
Types: deb
URIs: https://debian.koha-community.org/koha/
Suites: 25.05
Components: main
Signed-By: /etc/apt/keyrings/koha.asc
EOF
```

	5.  I exited the Linux root user through the `exit` command.
3. I installed MariaDB, which is a relational database that works a lot like MySQL:`sudo apt update` then `sudo apt install mariadb-server`
4. I installed the Koha ILS:
	1. `sudo apt update`
	2. `apt show koha-common`
	3. `sudo apt install koha-common`
5. I configured Koha to access both the staff interface (port 8080) and the public opac (port 8081) to match our earlier firewall rules.
	1. I changed my directory: `cd /etc/koha/`
	2. I ran `-ls` to see the files.
	3. I copied the `koha-sites.conf` to make a backup.
	4. I installed nano (`sudo apt install nano`) and opened `koha-sites.conf`to edit in nano. I set Intraport to 8080 and opacport to 8081. 
6. I enabled Apache modules:
	1. `sudo a2enmod rewrite cgi headers proxy_http`
	2. `sudo systemctl restart apache2`
	3. To ensure that Apache can respond to the 8080 and 8081 ports, I changed my directory: `cd /etc/apache2`
	4. I ran `-ls` to see the files.
	5. I made a copy of the `ports.conf` file.
	6. I used nano to edit the `ports.conf` file to add `Listen 8080` and `Listen 8081`.
7. I created a Koha library:
	1. `sudo koha-create --create-db bibliolib`
	2. I restarted Apache2.
8. I updated my Apache2 configurations:
	1. `sudo a2dissite 000-default`: /var/www/html is no longer the web doc root under this command
	2. `sudo a2enmod deflate`: turns on network compression
	3. `sudo a2ensite bibliolab`: enables the new bibliolib library I created in 7a.
9. I retrieved the username and password for the new bibliolib library:
	1. `sudo koha-passwd bibliolib`
	2. I wrote down the log in credentials.

10. I visited my staff site, logged in, completed the onboarding process, and added some Jane Austen titles to my library. I searched for them in the public opac. Success! 

## Important update

Thursday, April 16, was the first time I completed steps 1 through 10. I completed the install successfully!

The trouble started when I tried to log in to my WordPress admin account. I needed to do this in order to add the link to my Koha site on my WordPress site.
For some reason, I couldn’t access the wordpress admin account—it simply wouldn’t load! (I could access the public Wordpress site just fine!)

At first I thought it might be because of a bad internet connection. After all, it was working fine when I submitted my assignment last week. I thought it might be because of an internet connectivity issue, so I figured I’d give it a day and try again. 

When I tried to access the Wordpress admin account on Saturday, though, I didn’t have any more luck. I decided to take GoogleCloud’s “recommendation” (listed under the recommendation column on my VM instances page) to increase CPUs. 

Unfortunately, this seemed to make things worse. Now I saw a 404 message. The public Wordpress and the public Omeka pages were still fine. But stranger still, my Koha pages weren’t loading—even though I didn’t make any changes to my new Koha instance.

I tried a few different techniques I saw online to fix the 404 issue on the WP-admin page—clearing my cache, searching for my domain/wp-login.php, creating an .htaccess file, etc.—but still no luck.

Confused, I decided to create brand new VMs for both Koha and for Omeka/Wordpress on Sunday. The installs worked fine again, but I’m a little disappointed in myself for not having parsed what caused the issues in the first place. 

As I write this on Tuesday, April 21, I am once again unable to access my Koha site, even though I reinstalled it again on Sunday and it worked well! Luckily I’m not running in to the same issue with the wp-admin account. Will try to figure this one out.

