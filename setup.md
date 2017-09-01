# Server Setup

This guide will walk you through how to set up digital ocean and create your server.

## Digital Ocean

### Sign up or register for an account [here](https://www.digitalocean.com/).

Let's get started!

1. Create a Droplet
2. Select Ubuntu 16.04.2 x64 
3. Select server size ($5/mo)
4. Select datacenter region closest to you (New York)
5. Select Additional Options
	* You can select any option you want for this but for the sake of this tutorial we aren't selecting anything.
6. Add SSH Key
	* Here you can add one if you have it but if not we do that later in the tutorial. 
7. Lastly, hit the Create button.

Now let's start setting the server up!
There are a few tutorials that need to be followed for this but don't worry I have them here all in order.

## Initial Server Setup

We are going to take care of some intial setup here. This will increase the security and usability of your server.

### Step 1: Root Login

When you login into your server you need a few things. The public IP adress, the password for the root user or if you have a ssh key connected to the server. We haven't set up ssh yet so what is going to happen is we are going to login with the password.

`$ ssh root@your_server_ip`

##### The password will be emailed to you from Digital Ocean. 
Since this is the first time we are logging in we will be prompted to change the root users password.

#### It isn't a good idea to login as root. It is fine for the first time you login but now we need to make a new user.

### Step 2: Create New User

Now that we are logged in as root let's create a new user. We can do this by running this command. 
`$ adduser newUser`

You will be asked a few questions and then to create a new password. You don't have to answer the other questions. You can skip through them by pressing `ENTER`.

### Step 3: Privileges

Now we need to give this user some privileges. This is called making a 'superuser'. This is allowing the user you created to run `sudo` before a command to do admin task. 

`usermod -aG sudo newUser`

This is adding your new user to the sudo group. 

### Step 4: Add Public Key Authentication
#### THIS IS HIGHLY RECOMMENDED
##### Generate a Key Pair

This next section is assuming that you don't have a ssh key pair. If you have one then skip this step and go to the next one. 

`$ ssh-keygen`

You should get a output that looks like this: (assuming your local user is called "localuser")

> ssh-keygen output 

> Generating public/private rsa key pair.

> Enter file in which to save the key (/Users/localuser/.ssh/id_rsa):

Press `ENTER` to continue...

Next, you will be asked for a passphrase to secure the key with. You may either enter a passphrase or leave the passphrase blank. For the sake of this tutorial we are leaving it blank.

##### Copy the Public Key

There are two ways to do this:

1. This should work if you didn't set a ssh key when you set up the Droplet on Digital ocean.
`ssh-copy-id sammy@your_server_ip`
2. If you did set it through the droplet this is what you need to do:
	* On your Local Machine: Run `$ cat ~/.ssh/id_rsa.pub` to display your public key.
The output should look like this: 

> id_rsa.pub contents

> ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDBGTO0tsVejssuaYR5R3Y/
> i73SppJAhme1dH7W2c47d4gOqB4izP0+fRLfvbz/tnXFz4iOP/
> H6eCV05hqUhF+KYRxt9Y8tVMrpDZR2l75o6+xSbUOMu6xN+
> uVF0T9XzKcxmzTmnV7Na5up3QM3DoSRYX/EP3utr2+zAqpJIfKPLdA74w7/1rggpFmu3HbXBnWSUdf localuser@machine.local

Copy the public key (including the ssh-rsa part).

Now we need to let the new user use that ssh key to login.
We do this by adding the public key 
we just copied to a file that is in the home directory of the user.

Here is a quick command to run to switch users. Since we are currently logged in as root we need to now login as the newUser. 

`$ su - newUser`

You are now logged in as the newUser and in the home directory.

Create a new directory called .ssh and restrict its permissions with the following commands:

`$ mkdir ~/.ssh`

`$ chmod 700 ~/.ssh`

Now cd into the folder you just made and run this command to open the file that is in the ssh folder.

`$ nano ~/.ssh/authorized_keys ` 

Here is where you need to paste the public key that you copied. 

Hit `CTRL-x` to exit the file, then `y` to save the changes that you made, then `ENTER` to confirm the file name.

Now we need to restrict the permissions of the authorized_keys file with this command:

`$ chmod 600 ~/.ssh/authorized_keys`

Then exit...

`$ exit`

### Step 5: Disable Password Authentication 
#### THIS IS HIGHLY RECOMMENDED

Now that your new user can use SSH keys to log in, you can increase your server's security by disabling password-only authentication. Doing so will restrict SSH access to your server to public key authentication only.

> Only disable password authentication if you installed a public key to your user.
> 
> You will be completely locked out if you didn't!

Disable password authentication by running this command:

`$ sudo nano /etc/ssh/sshd_config`

In the file find the line that says PasswordAuthentication. Uncomment it by deleting the # and set it to 
> PasswordAuthentication no

Hit `CTRL-x` to exit the file, then `y` to save the changes that you made, then `ENTER` to confirm the file name.

### Step 6: Test Log In
#### Do not disconnect until you confirm that you can successfully log in via SSH.

Open up a new terminal window on your local machine. 
Login in as the newUser.

`$ ssh newUser@your_server_ip`

If you are able to login without entering a password then everything worked!

### Step 7: Firewall

We are setting up this firewall to make sure only connections to certain services are allowed.

Different services can register their profiles with UFW upon installation.

To see the services that are allowed to connect we run: 

`$ sudo ufw app list`

```
Output
Available applications:
  OpenSSH
```
  
We need to make sure the firewall allows SSH connections so that we can log back in next time. 

`$ sudo ufw allow OpenSSH`
`$ sudo ufw enable`

Type "y" and press ENTER to proceed.
Now check to see if everything worked by running this command:

`$ sudo ufw status`

The output should look a little like this:

``` 
Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
```


## Install Linux, Nginx, MySQL, PHP (LEMP stack)
[Learn more here](https://www.digitalocean.com/community/tutorials/how-to-install-linux-nginx-mysql-php-lemp-stack-in-ubuntu-16-04) 

### Secure Nginx with Let's Encrypt
It is recommended to do this but it is totally up too you.

[Let's Encrypt](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-16-04)

##  Install WordPress with LEMP
This tutorial will give you all the information you need to [install WordPress](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-with-lemp-on-ubuntu-16-04).

Start at Step 1 through Step 6.

##### Congrats! You have offical setup your server and you can now login to wordpress! 