---
layout: page
title: Defining volumes
---

## Exercise: create a volume 

###Solution: Dockerfile for Volume exercise

Create the Dockerfile in exercises/simple_node_service
```
FROM node_base
MAINTAINER Vincent Mos

ADD node_modules /opt/app/node_modules
ADD service.js /opt/app/service.js

EXPOSE 8099

CMD ["/usr/bin/nodejs", "service.js"]

```
Build the image

```
docker build -t simple_node_service .
```

Run the image
```
docker run -d -p 8099:8099 -v /home/dockdev/exercises/simple_node_service/data:/var/data simple_node_service

```

Note that you have to provide an absolute path to the directory you are
mounting.

Use `curl` or a browser to send a request to the service. Does it work?

