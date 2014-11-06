---
layout: page
title: Setting up
---

It is advised to use our provided Vagrant images, to let this
hands on lab work smoothly as explained below. You can download the VirtualBox 
image and VirtualBox install packages from our server (see below). 
We also provide USB sticks to copy the VirtualBox image and optionally install
VirtualBox first.

If you are for some reason unable to install either,
please pair up with someone else, and try it later at home.

## Find our network 

We provide a network for this lab, so that we are less dependent on the
conference wifi. Our network is (password 'helloqwan'):

```
QWAN
```

## Copy what you need 

Navigate to 

```
http://192.168.4.4/
```

and download:

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

The image is a basic Linux image with some necesseties installed and
some configuration prepared. The installed packages are:

* Docker (1.2.0) 
* Maestro-ng

It also contains some configuration needed to work effectively in a
conference setup. Docker images can be large, and we
transport them over a network connnection, so to ensure everything works
smoothly we have set up:

* a local network
* our own Ubuntu packages mirror
* our own Docker registry



