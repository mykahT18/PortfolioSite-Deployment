# PortfolioSite-Deployment
## Project Summary 
### This codebase is focusing on Setting up a server and installing Wordpress on that server. We will also cover the local installation and SCP (Secure Copy Protocol).

## Local Installation
#### Things you need to have:
1. Create a folder on your local machine: 
	* `$ cd ~/Desktop`
	* `$ mkdir wp-setup && cd wp-setup`
2. Download File from repo (Top right corner button).
3. Unzip the file and place it file into `wp-setup`.


The reason why we are creating this folder is to setup the environment you will be working in.

## Server Setup
[Server Setup Guide](https://github.com/mykahT18/PortfolioSite-Deployment/blob/master/setup.md)
If you don't have a server set up already here is a guide that will walk you through it. From setting up Digital Ocean to installing everything you need to get your server up and running.

## Deployment
#### Things you need to have: 
* Server Details
	* x1 Ubuntu 16.04 DigitalOcean Server
	* MariaDB (not Mysql)
	* Nginx
	* PHP
* Wordpress (Download from above.)

If you don't have these things setup on your server [here](https://github.com/mykahT18/PortfolioSite-Deployment/blob/master/setup.md) is a quick guide. 

#### This is just a example of how you deploy files from the local machine to your server using SCP. The human.txt file is the file we are going to deploy.

* The three most import things are:
	* The user credentials that you created on the server. (not root).
	*  The IP address of the server
	*  humans.txt 
		* Grab human.txt from here and save it on your local machine.

Once you have downloaded the human.txt file you need to use terminal to navigate to that location you saved it in. `$ cd whatever/location/human.txt`

Open the human.txt file to make sure it has all the content inside of it. You can do this in terminal by `$ nano humans.txt`

When the file opens you should see something like this as the first few lines: 
> /* Team */

> Name: Mykah Taylor

> Site: https://taylordev.me

Once you have checked to see if the file contains that content then we use SCP. What we are doing here is taking the human.txt file and deploying it to the server you made. 

`$ scp humans.txt username@your_ip:/var/www/html`

Now to check to see if this worked you can login to your server as your user that you created. Navigate to the folder where you put the file.

`$ cd /var/www/html && ls`

Here you should see the human.txt file in your directory.

Congrats you have now deployed using SCP!
	







 