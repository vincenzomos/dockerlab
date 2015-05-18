---
layout: page
title: Defining volumes
---

## Exercise: create a volume 

*Solution: Dockerfile for Volume exercise

```
FROM qwan/ubuntu_base
MAINTAINER Vincent Mos

RUN apt-get update && apt-get install -y nodejs npm

WORKDIR /opt/app

ADD node_modules /opt/app/node_modules
ADD service.js /opt/app/service.js

EXPOSE 8099

CMD ["/usr/bin/nodejs", "service.js"]

```

```
docker run -d -p 8099:8099 -v data:/var/data simple
```

Note that you have to provide an absolute path to the directory you are
mounting.

Use `curl` or a browser to send a request to the service. Does it work?

