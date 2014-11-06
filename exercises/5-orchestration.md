---
layout: page
title: Container orchestration
---

With Docker, you create containers that each encapsulate a single service or application. 
When you divide a system into a bunch of collaborating services, the services need to be able to access each other in a convenient way. 
Docker provides the concept of _container linking_ for this.

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
together. In other words, Docker falls short in orchestration.

There are several tools for Docker container orchestration, from simple
to complicated, from production ready to bleeding edge. We will use Maestro-
ng to illustrate automated container orchestration.

Maestro expects a yaml file that defines the different containers and
the way they are linked. 

Create a file `maestro.yaml`. We start with the first part of the file:

{% highlight yaml %}
name: WayTooComplicatedHelloWorld
ships:
   local: {ip: 'localhost', docker_port: 4243, timeout: 60 }
{% endhighlight %}

Make sure indentation and spaces are right, and don't use tabs! Maestro does
not provide very good error feedback.

A _ship_ is a maestro term for a machine that hosts containers. In this
exercise, we have one ship, localhost.

Try to run it:
```
maestro -f maestro.yaml status
```

The maestro _status_ command shows the current status of the involved containers.

What happens?

We need to define one or more services. Add the following to the yaml file:

{% highlight yaml %}
services:
   server:
      image: 'simple_node_service'
      instances:
          server:
              ports: { tcp: "8099" }
              ship: local
{% endhighlight %}

This defines one service called _server_ that is based on the
simple_node_service image. You also specify the exposed ports and the
machine (ship) on which the service will be run.

Run the status command again. What do you see?

Start the container using

```
maestro -f maestro.yaml start
```

Inspect the status; you can stop all involved containers with `maestro -f
maestro.yaml stop`.

Does the service work?

We forgot to specify the volume for the server! Add the following to the
definition of the server instance (put it just below the `ship:` line at
the same indentation level).

{% highlight yaml %}
services:
   server:
      # ....
          server:
              volumes:
                  /var/data: /home/vagrant/node_data
{% endhighlight %}

Now add the second service:

{% highlight yaml %}
services:
   # ....
   web:
      image: 'linked_node_app'
      instances:
          web:
              ports: { tcp: "8090" }
              ship: local
{% endhighlight %}

Start the containers. Whay happens?

We need to specify the dependency and the linking between the containers. 
Add the following to the web service definition, just below `ship:`, at the same level of indentation:

{% highlight yaml %}
services:
   # ....
   web:
      # ....
          web:
              # ....
              links:
                  server: service
              requires: [ server ]
{% endhighlight %}

Now try again...

