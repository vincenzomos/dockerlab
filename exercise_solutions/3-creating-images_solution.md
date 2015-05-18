---
layout: page
title: Creating images
---

## Exercise: define an image for a web application

*Solution: Dockerfile simple_node_app*
```
FROM qwan/ubuntu_base
MAINTAINER Vincent Mos

RUN apt-get update && apt-get install -y nodejs npm

WORKDIR /opt/app

ADD node_modules /opt/app/node_modules
ADD index.js /opt/app/index.js

EXPOSE 8099

CMD ["/usr/bin/nodejs", "index.js"]
```

Build command:

```
docker build -t simple_node_app .
```
## Exercise: extract a new Node.js base image

We want to define several service images based on Node.js. To prevent
duplication, we want to extract a common base image with Node.js in it.

- Define a new image that derives from ubuntu_base
  (in a separate directory)
- Add the node and npm packages to this image
- Build the image and tag it 'node-base'
- Update your existing Node.js based image so that it derives from
  node-base.
- Build and run that image


*Solution : Dockerfile node_base*
create a new dir for example : custom_nodejs_base.

Create a Dockerfile with the following content.

```
 FROM qwan/ubuntu_base
 MAINTAINER Vincent Mos

 RUN apt-get update && apt-get install -y nodejs npm
 ``
build the image :
```
docker build -t node_base .
```

*Updated simple_node image: Dockerfile*

```
FROM node_base
MAINTAINER Vincent Mos

WORKDIR /opt/app

ADD node_modules /opt/app/node_modules
ADD index.js /opt/app/index.js

EXPOSE 8099

CMD ["/usr/bin/nodejs", "index.js"]
```

