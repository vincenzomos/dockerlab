---
#  Defining volumes
---

## Exercise: create a volume 

Earlier on, you used Docker volumes to mount a local file in a
container. Now we are going to mount a directory that is used by a web
service. We have provide a Node.js based service that serves data from a file, in:

```
exercises/simple_node_service
```

If you inspect `service.js`, you will see that it serves the content of
a file in `/var/data`. We use volumes to mount a local directory in the
container.

- define and build a new Docker image for the service, based on node-base
- add the following instruction to the Dockerfile, which tells Docker
  to expose a port for linking:

```
EXPOSE 8099
```

- create a directory `data` and put a file `data.txt` in there; put
  whatever content you like in the file
- run the newly created Docker image with the -v option to mount your
  `data` directory as `/var/data` inside the container:

```
docker run -d -p 8099:8099 -v /absolute_path_to/data:/var/data your_image_name
```

Note that you have to provide an absolute path to the directory you are
mounting.

Use `curl` or a browser to send a request to the service. Does it work?


## Exercies: creating and using a data volume container


    docker run --volumes-from {container} training/postgress
