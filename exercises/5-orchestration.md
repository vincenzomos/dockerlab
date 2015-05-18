---
layout: page
title: Container orchestration
---

With Docker, you create containers that each encapsulate a single service or application. 
When you divide a system into a bunch of collaborating services, the services need to be able to access each other in a convenient way. 
Docker  provides the concept of _container linking_ for this.

## Exercise: linking two containers

We have provided a modified web application
that does an HTTP GET request to `http://service:8099` and passes the data on to the response. 
You can find the code for this application in 

```
exercises/linked_node_app
```

- create a Dockerfile for the web application and build the image
- run the node service from the previous exercise, and give it a name: '--name service'
- run the web application and link it to the service:

```
docker run -d -p 8090:8090 --link service:service your_web_app_image_name
```

Try it out. What happens?

The _--link_ option links another Docker container by name. That Docker
container will be accessible from within the other container through the
hostname _service_ (Docker modifies the hosts file in the container if
you run it with --link)

## Automate orchestration

Running linked containers together in the right order by hand is a bit
of a hassle. This becomes worse if you have many small services working
together. So we need something different to handle this. Docker Compose provides the tools for this

Using Compose is basically a three-step process. (Also see https://docs.docker.com/compose/)

1. First, you define your app's environment with a Dockerfile so it can be reproduced anywhere:
2. Next, you define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment:
3. Lastly, run docker-compose up and Compose will start and run your entire app.

For this exercise create a Dockerfile in exercises/compose_exercise
The Dockerfile actually is the same as the one for the previous exercise. 

Create the docker-compose.yml. Underneath is an example of a web app using a database (From the documentation).

```
web:
  build: .
  links:
   - db
  ports:
   - "8000:8000"
db:
  image: postgres
```
Now you have to figure out how to connect the webapp to the service. 

Afterwards run docker-compose-up. 


