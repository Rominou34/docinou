# Vagrant

Vagrant is a good software allowing you to quickly and easily create and run virtual machines accessible via SSH.

**Fun fact:** The first time I installed it, I tested it a little bit and found it difficult to use. I didn't understand why my virtual machine wasn't responding when I was sending requests to the specified address ( 127.0.0.1:PORT ), but I didn't even install Apache :s

## Installing Vagrant

Go [there](https://www.vagrantup.com/downloads.html) and download the version you need.

I won't write how to install it, I don't think you need a tutorial for that :p

## Starting your project

#### 1. Initializing the workspace

Before starting working, you need to prepare your workspace, it will be a folder dedicated to your virtual machine. So just create a folder, go into it and initialize it for Vagrant:

```bash
$ mkdir project
$ cd project
$ vagrant init
```

This will create a `Vagrantfile` file inside the folder. It's a config file, similar to `.env`, or `.yml` files.

#### 2. Installing a box

After having initialized your directory for Vagrant, you're ready to install your box. Choose the one you want from [the Vagrant catalog](https://atlas.hashicorp.com/boxes/search), and run the `vagrant box add` command with the corresponding box.

For example, if you wish to install ubuntu/trusty64:

```bash
vagrant box add ubuntu/trusty64
```

Then you need to configure Vagrant to use this box, go into your `Vagrantfile` and change the corresponding line with the box you just added:

```
config.vm.box = "base" // Initial
config.vm.box = "ubuntu/trusty64" // New value
```

#### 3. Running your box and SSH-ing into it

Now that your box is installed, use it :D

Go into the workspace directory of your box ( here it is `project` ) or just stay there if you were already in it, and run the following command:

```
vagrant up
```

Then VirtualBox / VMWare will launch in the background and your box will start running

Now just connect into your box:

```
vagrant ssh
```

**NOTE:** Sometimes this command doesn't work, and Vagrant tells you to specify the id of your box. If so, just do this:

```
vagrant global-status
```

to get all your machines ids and then

```
vagrant ssh *MACHINE_ID*
```

#### 4. Synced folders

I didn't understand any of this shit so I'll do it one day if I ever understand it

#### 5. Using your box

Congrats !! You're now connected to a virtual machine via SSH, you can now fool around with it.

The only thing is that your box is completely empty ( as proof you land inside the `/home` folder when you ssh and it's perfectly empty )

So first things first, we need to install Apache:

```bash
sudo apt-get update
sudo apt-get install apache2
```

Now Apache is up and running on your awesome virtual web server, but you're still not able to see it, we're almost there !!

#### 6. Networking

The easiest way to set up the network of your virtual server is port forwarding.

To do it, it's easy, just go into your `Vagrantfile` in your workspace repository ( in your computer, not in the virtual server ;) ) and add this line:

```
config.vm.network :forwarded_port, guest: 80, host: 4567
```

Now, your virtual server is reachable through `http://127.0.0.1:4567` ( if you type this into your browser and everything went well, you should see apache's welcome message )
