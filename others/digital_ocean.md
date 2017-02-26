# Digital Ocean

Some documentation about creating an Ubuntu 16.04 droplet on Digital Ocean and installing the LAMP stack on it ( without the one-click app )

## 1. Creating a SSH keypair and setting it up on Digital Ocean

### 1.1 Generating the keypair

1. Go into the .ssh folder where keys are stored:
```
cd ~/.ssh
```

2. Generate a keypair:
```
ssh-keygen -t rsa
```
Set a passphrase for increased security if you want to, or just beware to never share your private key

Choose whatever name you want ( I chose `dig_ocean` so we'll use this for the example )

### 1.2 Adding the key to your account

Now go to your account ( 2 choices possible ):

* Click on your profile -> Settings -> Security and add it there
* Add it when creating the droplet

To add your key, copy paste the content of `dig_ocean.pub` into the field and give it a name ( `dig_ocean.pub` and not `dig_ocean`, this is very important )

## 2. Creating the droplet

Digital Ocean's interface is very intuitive and simple, so you can easily create the droplet

* Choose either "Distributions" or "One-click apps". One-click apps allow you to create a server with a software already installed and running on it ( Wordpress, Git, LAMP, etc. ) while "Distributions" will create a server running only Linux.

**For the sake of this tutorial, we'll use a basic server with only Linux, so choose "Distributions"**

* Choose your OS ( Ubuntu and Debian being the most used so you will have help more easily if you choose these ). **In this tutorial, we're going to use Ubuntu 16.04**

* Choose the size ( I chose the cheapest one at 5$/mo because I'm broke )

* Add block storage: This options allows you to ad an additional disk on your server if you really need some storage. This is only available on some regions and I don't need one so I didn't chose any

* Choose the datacenter where your VPS will be located. I chose AMS2 ( Amsterdam 2 ) as I live in France

* Choose additional options:
  * Private networking allows you to have a LAN connection between droplets in the same datacenter **( useless as we'll only create one droplet in this tutorial )**
  * Backups: For 20% more price, you can enable weekly backups of your droplet. I didn't enable this **( you can enable it later if you change your mind )**
  * IPv6: Enables IPv6 networking, I didn't enable it either ( you can also enable it later )
  * User data: I don"t know what it does, sorry
  * Monitoring: allows you to have graphs showing live graphs CPU, MEmory, Disk I/O, Disk usage, Public bandwith and top processes of your doplet. I enabled it.

* Add your SSH key:
  * If you already added it, choose it from the list
  * If you didn't, see step **1.2**

**If you don't add any, Digital Ocean will generate a random password and e-mail it to you. You'll then be able to login as root in your droplet and eventually add yur SSH key later, but using SSH keys is far more secure so I heavily recommend it to use SSH keys**

* Choose if you want to create multiple droplets with this config or only one, and name it so you can recognize it in your admin interface

* Then create the droplet and wait for Digital Ocean to complete the installation, then proceed to the next step

## 3. Connecting to your droplet and securing it

### 3.1 Connecting

To connect to your droplet, use the `ssh` command on your droplet IP:

```
ssh root@[droplet_ip]
```

If you didn't choose the default file name for your keypair ( as I did because I named it `dig_ocean` ), you'll need to specify ssh which key to use:

```
ssh root@[droplet_ip] -i [key_name]
```

In my case, it will be:
```
ssh root@[my_droplet_ip] -i dig_ocean
```

If everything went fine, you'll be logged into your droplet as root.

### 3.2 Securing the droplet

Now that your SSH connection using keys is working, remove the ability to connect using a root password. This way, the only way to connect to your droplet is to have a keypair registered on the server:
```
vim /etc/ssh/sshd_config
```

Use this command to edit the file using vim ( you can also do it with nano )

Then find the line PermitRootLogin and change its value:
```
PermitRootLogin without-password
```
Now kill the sshd process to make it restart, taking your change into account:
```
ps auxw | grep ssh
```
This will show the current ssh processes, now use `kill -HUP [PID]` to kill each one containing `sshd` in COMMAND to kill them ( `[PID]` being the value in the PID column )

# X. Adding subdomains

I'm gonna write it later, for now these 2 tutorials should help:

[Creating the virtualhost](https://www.digitalocean.com/community/questions/how-do-i-setup-subdomains-for-my-droplet?answer=1621)

[Redirecting it to a folder](https://www.digitalocean.com/community/tutorials/how-to-set-up-apache-virtual-hosts-on-ubuntu-12-04-lts)

# Enabling HTTPS

https://community.letsencrypt.org/t/always-redirect-to-https/10838/9

https://certbot.eff.org/#ubuntuxenial-apache

http://askubuntu.com/questions/184791/how-to-disable-non-ssl-connection-on-apache-2-2
