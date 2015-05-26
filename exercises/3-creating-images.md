---
# Creating images
---

## Exercise: define an image for a web application

We have provided a very simple web application running on Node.js, in

```
exercise/simple_node_app
```

#### Steps to define a Docker image for this web application:

- in the web application directory, create a file called `Dockerfile`

- specify the base image from which your image derives

```
FROM sogeti:5000/ubuntu
```

- optionally, specify the author of the image

```
MAINTAINER your_name_goes_here
```

- install required packages; in this case, 
use apt-get to install `nodejs` and the node package manager `npm`;
first make sure you have the latest list of packages

```
RUN apt-get update && apt-get install -y nodejs npm
```

Note that the RUN instruction will be executed when you _build_ the image

- specify the working directory for the application or service

```
WORKDIR /opt/app
```

- copy all required files to the image; in this case, the required node
  modules are in directory `node_modules`, we copy them to the working
directory

```
ADD node_modules /opt/app/node_modules
```

and add the application itself:

```
ADD index.js /opt/app/index.js
```

- specify the default command that will be run when a container is started from the
  image

```
CMD ["/usr/bin/nodejs", "index.js"]
```

Note: this command is run when you run the container without specifying
a command. If you run the container with e.g. /bin/bash, the default
command will not be run.

The container will keep running as long as the command has not finished.

Now you can build the image using 

```
docker build -t simple_node_app .
```

`simple_node_app` is the tag of your image.

Now run the image as a container with:

```
docker run -d -p 8090:8090 simple_node_app
```

See if it works with `curl`:

```
curl -v localhost:8090
```

Leave out the -v so see just the response body. 
**note**:  it will be on one line with the command prompt

How can you see if the container is running? Take a look at the
container's logs.

Stop the container.

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



