# LIS_624
Systems Librarianship

## 2026-02-09 - Systems Librarianship

Goal: To understand how GitHub works

Context: I'm taking the LIS 624 class with Dr. Burns.
 
Steps: I'm listening to the lecture as I work.

Results: I've successfully pushed changes from nano to my repository

Verification: I see the update in GitHub.

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

## 2026-02-23 - Managing Software (6)

The `sudo` command modifies files, creates directories, and performs maintenance tasks needed to install and manage software.

`sudo apt update` = compares the software that's available to what's installed on my system
* When a piece of software is available to be updated, run the `sudo apt upgrade` command

