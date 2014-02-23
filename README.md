I created this guide because I wanted to have a docker container on Ubuntu that ran a Node.js server.

Initial problems I ran into before creating this guide:

 - The Docker documentation for this doesn't make much sense, is for a *very* old version of Docker, and uses CentOS.
 - The Docker daemon only works on linux and not OS X or windows - I setup Vagrant for this

# Getting Started
You will need…
- VirtualBox - https://www.virtualbox.org/
- Vagrant - http://www.vagrantup.com/

For Windows - you will also need
- ssh client (eg. Putty - http://www.chiark.greenend.org.uk/~sgtatham/putty/)

## Initialize Vagrant
1. Run this command from the project's root:

    vagrant up

It will begin loading up the VM; during the initialization, it will ask you which network interface you wish to connect to; 
make sure to choose one that is exposed to the internet (eg. Wifi, Ethernet).

2. Once it is done, ssh into the vagrant instance:

    vagrant ssh

For Windows - If you are using windows and run this command you may run into an issue saying that an ssh client hasn’t been configured. If you’re using Putty you’ll need to use PuTTYgen.exe to convert the vagrant ssh key to .ppk file which you can then use in Putty to remote in.

If you get an error about Guest Addition versions not matching, first try:

    ls /vagrant

and if you see the contents of this project there, then you're good to go. Vagrant will share the host directory contents that has `Vagrantfile` as `/vagrant` in the VM.

## Modify UFW
Ubuntu's default UFW will drop incoming connections - to fix this

    sudo nano /etc/default/ufw

    # Change:
    # DEFAULT_FORWARD_POLICY="DROP"
    # to
    DEFAULT_FORWARD_POLICY="ACCEPT"

Save the file and then run:

    sudo ufw reload

## Creating a docker container

    cd /vagrant

    sudo docker build -t <your-name>/node .

The `.` at the end means take all files in the directory and add them to the container to be built.

## Running a docker container

    sudo docker run -d -i -t baseman/node /bin/bash

You can retrieve the running container id by typing:

    sudo docker ps

And then attach to the instance:

    sudo docker attach <container id>

You can detach and stop the container instance by typing ‘exit’

## Getting the container's logs

First, view the docker containers:

    sudo docker ps

Find the `CONTAINER_ID` by typing the following command:

        sudo docker ps

And then run:

    sudo docker logs CONTAINER_ID_HERE

### Additional UFW ports
If you need to open additional ports, add:

    sudo ufw allow <port number>/tcp

### Other Docker Commands

- List all containers

    sudo docker ps -a -q

- Remove all containers

   sudo docker rm [$(sudo docker ps -a -q)]

