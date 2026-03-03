# LIS_624
Systems Librarianship

## 2026-02-09 - Systems Librarianship

Goal: To understand how GitHub works

Context: I'm taking the LIS 624 class with Dr. Burns.
 
Steps: I'm listening to the lecture as I work.

Results: I've successfully pushed changes from nano to my repository

Verification: I see the update in GitHub.

### Edits in my repository
Don't forget to change directories in order to edit my `README.md` file:
* To access this file: `cd LIS_624`

## 2026-02-10 - An update

I still can't figure out why ssh authentication kept failing for my first repository, so I've created a new one. As a reminder, here are the commands to use in order to push changes from my text editor to this repository:

* `git add name_of_entry.md`
* `git commit -m "commit message here"`
* `git push origin main`
* `git status`

## 2026-02-10 -  Module 2 Check-In

Here's how I'm using my repository so far:

* To take note of commands I'll return to
* To document my progress throughout the course
* To make changes/edits in nano and to check how they would appear if published on GitHub 

## 2026-02-16 - Searching with grep (5)

grep commands to return to:

`grep "example" file_name.csv` = search for the string in quotation marks

`grep -i "example" file_name.csv` = -i ignores the case of the search string

`grep -vi "example" file_name.csv` = -vi searches for lines that don't match the exact string (called inverse matching). -i is added to ignore the case

`^` = indicates the start of a line, ergo 

`grep -vi "^example" file_name.csv` = searches for lines that don't match the string in quotation marks at the start of a line

`$` = indicates the end of a line, etgo

`grep -vi "example$" file_name.csv` = returns lines that don't end with the string in quotations

`c` = count of matching lines

`ic` = case insenstive count of marching lines

To be continued

## 2026-02-23 - Managing Software (5)

The `sudo` command modifies files, creates directories, and performs maintenance tasks needed to install and manage software.

`sudo apt update` = compares the software that's available to what's installed on my system
* When a piece of software is available to be updated, run the `sudo apt upgrade` command

## 2026-02-23 - Library Search (6)

To search for a software to install: `apt search name of software`
* For YAZ: `apt search yaz`
* Before automatically installing whatever comes up, run `apt show name of software` to get information about the program 
* To complete the installation: `sudo apt install name of command`
*	For class: `sudo apt install yaz`

To run the YAZ program: `yaz-client`
* A new command line interface appears: `Z>`
*	With this interface, I can run the `open` command to connect to a library OPAC or discovery service using the server address
*	`Z> open saalck-uky.alma.exlibrisgroup.com:1921/01SAA_UKY`

### Queries

Prefix Query Format (PQF)

* `find` sends a search request (also can be abbreviated as `f`
* After command, use an operator to do a Booelean search of the next two attributes (e.g., `Z> f @and` `Z> f @or`)
* Use `@attr 1=attribute` 
* Here are examples of searches I constructe with PQF:
	* `f @attr 1=1 "batuman, elif"`
	* `f @and @attr 1=31 "2026" @attr 1=21 "artificial intelligence"`
* For a list of attributes, see [Library of Congress](https://www.loc.gov/z3950/agency/defns/bib1.html)

## Module 3: Check-In

I'm using my repository to remember commands that I return to often. For example, I haven't yet memorized the four commands to push changes from my text editor to my repo, but my 2026-02-10 entry provides the commands for easy reference.

## 2026-03-02 - Installing the Apache Web Server

### What is a server anyway?
A web server (like Apache, for instance) stores files. When a web browser requests access to retrieve or display a web server’s files, the server can successfully provide access to those files—or the server can indicate the error preventing access.
If a web server provides access to files, the document root indicates where files are stored on the system. It acts as a directory for the server’s files.

### Steps for installation
1. Per usual, update my system first. I want to install the latest and most secure version. (Command: `sudo apt update`)
2. Search for the package name: `apt search apache2`
3. If i want to make sure I have the right package, run `apt show <package_name>`
4. Install the package: `sudo apt install apache2`

#### Reminder: What's sudo?
We use the `sudo` command to execute a command as a "superuser," which is the root account.
We use it when we're working on files outside of our home directories.

### Is this thing on?
Make sure `apache2` is actually running: `systemctl status apache2`
* When I run this command, I see that the software is "active." That's what I want.

### Creating a web page
1. We're using a command line browser to engage with the web page I'm creating on my Apache server. I went with `elinks`, and you know the drill: `sudo apt install elinks`
2. Then we ran `elinks localhost` but I'm not quite sure I fully understand this. Come back to this one!
3. I can visit by page on a graphic browser (I use Brave) but getting my public IP address from my VM. 
4. To continue on, I need to change my directory to `/var/www/html` because that's the location of my document root (see above!).
5. IMPORTANT! Most web servers look for a file called `index.html` as the "default entry point." Let's backup what's there by moving into another file: `sudo mv index.html index.original.html`
6. Ok. Now `index.html` is what I'm going to edit.: `sudo nano index.html`
7. I added some HTML based on Dr. Burns' template code.
8. When I visit my public IP address, I see what I've added to `index.html` by way of my public IP address (which defaults to `IP address/index.html`
9. Tada!
