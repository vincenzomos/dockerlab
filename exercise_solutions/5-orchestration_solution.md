---
layout: page
title: Container orchestration
---

With Docker, you create containers that each encapsulate a single service or application. 
When you divide a system into a bunch of collaborating services, the services need to be able to access each other in a convenient way. 
Docker Compose provides the concept of _container linking_ for this.

*#1. Create Docker file*
````
FROM qwan/ubuntu_base
MAINTAINER Vincent Mos

RUN apt-get update && apt-get install -y nodejs npm

WORKDIR /opt/app

ADD node_modules /opt/app/node_modules
ADD linked.js /opt/app/linked.js

EXPOSE 8099

CMD ["/usr/bin/nodejs", "linked.js"]
```

*#2. Build image*
docker build -t link-service .

*#3. Run the service from exercise 4*
docker run -d -p 8099:8099 --name simple-node-process -v  /home/dockdev/exercises/simple_node_service/data/:/var/data simple_node_service

*#4. Run the linked docker*
docker run -d -p 8090:8090 --link simple-node-process:service link-service

*#5. Open your browser to localhost:8090*

*Docker orchestration*

You can reuse the Dockerfile

 The yaml file should contain :

 ```
 composeServiceDemo:
   build: .
   ports:
    - "8090:8090"
   links:
    - dataService
 dataService:
   image: simple_node_service
   ports:
    - "8099:8099"
   volumes:
    - data:/var/data
 ```

 The ports in composeServiceDemo link the container's port to the Docker Host.

 The ports in dataService link the simple_node_service port to the composeServiceDemo.