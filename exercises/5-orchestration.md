---
#  Container orchestration
---

With Docker, you create containers that each encapsulate a single service or application. 
When you divide a system into a bunch of collaborating services, the services need to be able to access each other in a convenient way. 
Docker  provides the concept of _container linking_ for this.

## Exercise: linking two containers

We have provided a wordpress application

You can find the code for this application in 

```
exercise/wordpress

```		

- extract the wordpress sources _sources/latest.tar.gz_
- copy the _router.php_ and _wp-config.php_ files from the sources to the wordpress directory
- Create a Dockerfile for the wordpress application. Base it on the _sogeti:5000/php5_ image. Add the '.' directory as '/code'
- Build the wordpress image


And start wordpress
```
docker run -d -p 8080:8080 wordpress_image_name php -S 0.0.0.0:8080 -t /code
```

Go to localhost:8080...
however, you will get: 
**Error establishing a database connection**

Why? Because it has no database. The postgresql db needs to be linked to the worpress image

Stop the worpress container and start the database.

Create a container for a db:

```
docker run -d --name mydb -e MYSQL_DATABASE=wordpress sogeti:5000/mysql
```

Now start the wordpress again, but this time link it to the database: 

```
docker run -d -p 8080:8080 --link mydb:db wordpress_image_name php -S 0.0.0.0:8080 -t /code
```

Try it out. What happens?

The _--link_ option links another Docker container by name. That Docker
container will be accessible from within the other container through the
hostname _service_ (Docker modifies the hosts file in the container if
you run it with --link)

## Automate orchestration

Running linked containers together in the right order by hand is a bit
of a hassle. This becomes worse if you have many small services working
together. So we need something different to handle this. *Docker Compose* provides the tools for this

Using Compose is basically a three-step process. (Also see https://docs.docker.com/compose/)

1. First, you define your app's environment with a Dockerfile so it can be reproduced anywhere:
2. Next, you define the services that make up your app in docker-compose.yml so they can be run together in an isolated environment:
3. Lastly, run docker-compose up and Compose will start and run your entire app.

For this exercise create a Dockerfile in exercise/wordpress
The Dockerfile actually is the same as the one for the previous exercise. 

Create the docker-compose.yml. Underneath is an example of a web app using a database (From the documentation).

    web:
      build: .
      command: /bin/bash
    links:
      - db
    ports:
      - "8000:8000"
    volumes:
      - .:/my_path
    db:
      image: a_db_image
      environment: 
        MY_KEY: my_value


Now you have to figure out how to connect the wordpress to the service. 

Make your changes and afterwards run : 
```
docker-compose-up 
```

