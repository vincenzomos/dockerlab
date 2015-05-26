---
# Running containers
---

Open up a terminal (pressing your windows key and start typing
'terminal' should help you find it) and type:

```
docker ps
```

If that returns:

```
CONTAINER ID    IMAGE      COMMAND     CREATED STATUS      PORTS       NAMES
```

You're ready to start.

_docker ps is like the Linux ps command and shows all running Docker
containers_

## Running 'Hello Docker' and understanding what happens

Type:

```
docker run -it sogeti:5000/ubuntu /bin/echo "Hello Docker"

```
At the bottom of your output it shows:

*Hello Docker* 

Nice huh?!

## What happened?

exited the terminal, stopped the container, and removed it.

First, the image was pulled from the (local) dockerhub and you saw something like this. 

```
maestro@mocker:~$ docker run -it sogeti:5000/ubuntu /bin/echo "Hello world"
Unable to find image 'sogeti:5000/ubuntu:latest' locally
Pulling repository sogeti:5000/ubuntu
07f8e8c5e660: Download complete
e9e06b06e14c: Download complete
a82efea989f9: Download complete
37bea4ee0c81: Download complete
Status: Image is up to date for sogeti:5000/ubuntu:latest
```
After that, Docker created a container from the image, fired it up, attached a terminal to it, and ran

```
/bin/echo "Hello Docker" 
```

In this case the image was pulled from a local repository. 
You can of course also do this from the internet by running the same on an image that isn't provided by the local repository. (You can find thousands on dockerhub).
But keep in mind it might be slow. 


### The output explained further

The first two lines of the output are straightforward. 

```
Unable to find image 'sogeti:5000/ubuntu:latest' locally
Pulling repository sogeti:5000/ubuntu
```

The image you are trying to start as container, is not available. It
needs to be fetched from the repository. Note the Docker calls the complete path the repository.

Then there are 4(!) lines saying:

```
<some number> Download complete
```

Why? Well, Docker stores snapshots of images called _layers_, similar to
the way Git stores commits. These layers emerge in the creation process of
containers. We'll learn more when we start creating our own containers.

So the Docker image has been downloaded. That means if you repeat the command
we executed earlier, it will run be faster. Try it and observe how fast
it is.

To see the Docker images, type:

```
docker images
```

and

```
docker images -a
```

Explain the difference in output.


### The Docker run command line explained further

The _-i_ option opens a stdin to the container's running process and
_-t_ associates a pseudo TTY to the container. Both -i and -t are often used
together when you want to run a container in the foreground (i.e.
not detached). Docker containers are often run in detached mode rather than in the
foreground.

_/bin/echo "Hello Docker"_ is the command being executed by the Docker
container. Docker containers are often used to run one specified command,
typically a service. The command to run can be overridden on the command
line the way we did.

Now try to run the same command line but replace _/bin/echo ..._ by
_/bin/bash_ like:

```
docker run -it --rm sogeti:5000/ubuntu /bin/bash
```

Notice that you are logged in to some terminal as root, indicated by:

```
root@<container id>/#
```

It appears that you have started a complete virtual machine. Wander
around, look at the directory tree, the hostname. It really feels like a
virtual machine. Is it actually one? 

No not really. It is a little bit like an advanced way of doing chroot.
It feels like a virtual machine though. It has its own virtual file
system (which is actually a directory structure on the host, like
you'd get with chroot), it has its own virtual network interfaces
(observe that ifconfig yields different results than ifconfig on
your host). 
So it feels like a virtual machine,
but a really lightweight one, without need for hypervisoring. It is just
a few processes in a, eh _container_.

To see what is running, open another terminal (ctrl-shift-n) and type:

```
ps aux | grep bash
```

There are a few bash-es running but take a close look at these:

```
maestro   3049  0.0  1.1 116296 12184 pts/13   Sl+  18:27   0:00 docker run -it --rm sogeti:5000/ubuntu /bin/bash
root      3057  0.0  0.3  18180  3152 pts/24   Ss+  18:27   0:00 /bin/bash
```

There is a Docker process owned by you and a bash process owned by
root. Notice the similarities when you do 'ps aux | grep bash' in the
container.

So here we have a kind of virtualization that starts up instantly and is
really lightweight.

Exit the container with 

```
 < ctrl-d >
```

to prepare for the next next exercise.

## Running a detached container and inspecting it

Let's start the same command we saw earlier, but in a loop and detached.
Type on one line:

```
  docker run -d sogeti:5000/ubuntu  /bin/bash -c 'while true; do echo "Hello Docker"; sleep 2; done'
```

The only output you get is a hash of the started container. 

## Looking at running containers
Look at:

```
docker ps
```

You will see two containers running: a container called 'registry', which we have installed to provide you with a local docker registry, and, your container.

The name of your container is generated by Docker. In my case it is
condescending_hypatia.

## Look at resources used. 
Since Docker 1.5 it's possible to see the resources used by your running containers. For this the stats command was introduced. You use it like this:
```
docker stats condescending_hypatia
```


## Looking at 'logs'

To see the output of your process, Docker logs attaches a standard out
to your terminal. This allows you to look at the logs. When using
Docker containers for processes, just logging to standard out is
often used for logging, as it integrates with Docker logs.

Try:

```
docker logs condescending_hypatia
```

(replace 'condescending_hypatia' with your container name)


it should show a whole list of:

```
...
Hello Docker
Hello Docker
Hello Docker
...
```

Adding the _-f_ option follows the log. Type a few enters to actually
see anything moving if your screen has already been filled with 'Hello Docker'
messages.

## Stopping and starting the container

Open another terminal and type:

```
docker stop condescending_hypatia
```

(replace 'condescending_hypatia' with your container name)

After a few seconds you'll see that the container actually stops running.

Restarting the container can be done with 

```
docker start condescending_hypatia
```

and

```
docker run -d sogeti:5000/ubuntu  /bin/bash -c 'while true; do echo "Hello Docker"; sleep 2; done'
```

Try both and explain the difference with 'docker log' and 'docker ps -a'.

## Passing environment variables

We can pass environment variables to a Docker container with the _-e_
option to the run command. For example:

```
docker run [other options] -e MESSAGE="Hello Environment" <container name> [command]
```

passes the MESSAGE environment to the Docker container. Go ahead and check
this by starting bash as command and use `env` or `echo $MESSAGE` to check
its existence.


## Defining port forwards

We have a container with an Apache webserver. Let's fire it up:

```
docker run -d  sogeti:5000/php5  /usr/sbin/apache2ctl -D FOREGROUND

```

Verify that it is running with:

```
docker ps
```

Now open up a browser and browse to:

```
http://localhost:80/
```

It doesn't work, does it? The problem is that, while the web application is
listening to port 80, your host is not forwarding the port.

```
docker run -p <host:port>:<forward port> ....
```

forwards a port to your container. Now try and run the container
again while forwarding **80** on your localhost to **80** on your container
and try browsing to the web server again.

And Yay!!! it works. It shows the default Apache2 webpage.

Look at the output of `docker ps` and see the port forwarding in the
output.

There are several ways of having a network of containers working
together, as we will see later on. Using port forwaring and knowing the host on which you run is
one of them.

## Defining a volume

"Where is my data ???" - sooner or later you'll ask yourself this
question when working with Docker containers. In a way we would say "why
should you care": it does not really matter where data resides, as long as it is
persistent and both your application and a backup system can access it,
you're fine.

One thing you do not want, is that by stopping a container and
running it again, you would destroy or hide your data.

Volumes comes to the rescue here. 
Volumes can be defined in the Dockerfile as well as in the run command.
We will focus on defining them in the run command here.

With the run command you can use _-v_ to define a volume for a container.
Lets try.

Create a file 'data.txt' in the current directory with some interesting
content. Run the container by typing (on one line):

```
docker run -it --rm -v `pwd`/data.txt:/var/data.txt sogeti:5000/ubuntu /bin/bash -c 'cat /var/data.txt'
```

_Note: the `pwd` (Print Working Directory) substitutes the absolute path of the current directory;
you need an absolute path for the volume._

This mounts your file as `/var/data.txt` in the container. You can also
mount a directory. In this way, data can survive the container.

## Let's clean up our mess

### Clean up containers a

First stop the container(s) that we have started. Make sure that
```
docker ps
```
yields and empty list. So there are no containers left.

But aren't there really?

Try:
```
docker ps -a
```

This shows all the containers we have started earlier with information that
they exited earlier. 

```
docker stop <container hash or name>
```

stops a container and 

```
docker rm <container hash or name>
```

deletes a container. Knowing that

```
docker ps -a -q
```

shows just the hashes of all containers, what would be a fast way to
remove all containers from your host?

Clean them up now, we'll move on to images.

### Cleaning up images

We've seen the `docker images (-a)` command. Docker images has a _-q_
option as well, which helps you to swiftly remove all images. 

You should not remove the containers now. We can use these in the rest of the workshop.


