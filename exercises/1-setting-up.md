---
# Setting up
---

It is advised to use our provided images, to let this
hands on lab work smoothly as explained below.

We also provide USB sticks to copy the VirtualBox image and optionally install
VirtualBox first.

If you are for some reason unable to install either,
please pair up with someone else, and try it later at home.

## Copy what you need 

download:

* VirtualBox (if needed)
* Our HandsOnDeployment image

## Install VirtualBox

Install VirtualBox if you haven't got it installed on your laptop.

* [Windows download](/materials/VirtualBox-4.3.18-96516-Win.exe)
* [Mac OS X download](/materials/VirtualBox-4.3.18-96516-OSX.dmg)
* [Linux all distributions download](/materials/VirtualBox-4.3.18-96516-Linux_amd64.run)

Now download the [virtual box image](/materials/qwan-docker-lab_default_1415141114072_69666.ova)

Then fire up the image: double clicking the image should import it in
VirtualBox, where you can then start it.

The image is a basic Linux image with some necessities installed and
some configuration prepared. The installed packages are:

* Docker (1.6.0)
* Docker Compose (1.2.0)



### Configuration

- Username: maestro
- Password: maestro


**Configuring virtual box ssh:**
 
Settings > Network (NAT) > Configure ports >


- Name: ssh
- Protocol: TCP
- Host IP: 127.0.0.1
- Host Port: 2222
- Guest Ip: 
- Guest Port: 22


Now you can connect to the machine using a local ssh client (e.g. PuTTy), using
 
    localhost:2222


**Running vm in background**

If you press left-shift while starting the vm, the vm will run headless. Great if you only want to interact with by using ssh.