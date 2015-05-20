---
layout: page
title: Running containers Solution
---

To see the Docker images, type:

```
docker images
```

and

```
docker images -a
```

#### Explanation of  the difference in output.
*docker images* will show all images in a completed state. That is after all modifications were done on an image.
*docker images -a*  will show all intermediate images as well. For each instruction in a Dockerfile an image will be tagged with an ID.

## Stopping and starting the container

Open another terminal and type:

```
docker stop condescending_hypatia
```

(replace 'condescending_hypatia' with your container name)

After as few seconds you'll see that the container actually stops running.

Restarting the container can be done with 

```
docker start condescending_hypatia
```

and

```
docker run -d qwan/ubuntu_base /bin/bash -c 'while true; do echo "Hello Docker"; sleep 2; done'
```

Try both and explain the difference with 'docker log' and 'docker ps'.

####Explanation : 
The first one just resumes where it stopped the first time. The latter one will start a new container.

## Passing environment variables

Solution for showing MESSAGE

```
docker run -it --rm qwan/ubuntu_base /bin/bash -c env
```

## Defining port forwards

```
docker run -p <host:port>:<forward port> ....
solution
docker run -d -p 8080:8080 qwan/helloworldapp

```

And Yay!!! it works.

Look at the output of `docker ps` and see the port forwarding in the
output.

There are several ways of having a network of containers working
together, as we will see later on. Using port forwaring and knowing the host on which you run is
one of them.

## Defining a volume


Create a file 'data.txt' in the current directory with some interesting
content. Run the container by typing (on one line):

```
docker run -it --rm -v `pwd`/data.txt:/var/data.txt qwan/ubuntu_base /bin/bash -c 'cat /var/data.txt'
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

####Solution : for easy removal
```
docker rm $(docker ps -a -q)
```

### Cleaning up images

We've seen the `docker images (-a)` command. Docker images has a _-q_
option as well, which helps you to swiftly remove all images. 

You should not remove the images now. We can use these in the rest of the workshop.


