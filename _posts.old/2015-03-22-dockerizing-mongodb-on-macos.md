---
layout: post
title: "Dockerizing MongoDB on MacOS"
description: ""
category: linux
tags: [macos, linux, docker, mongo]
---
{% include JB/setup %}



# HOWTO - MongoDB in a Docker container on MacOS 10.10 Yosemite

This is a very short list of the steps to get a MongoDb running in a Docker container.
I have simplified the steps into a simple startup script, but there is no magic here, just a 
bit more convenience.

# Onetime setup

- get [docker](https://docs.docker.com/installation/mac/) - currently version 1.5
- git clone this repository: [abarbanell/mongodb.docker](https://github.com/abarbanell/mongodb.docker)

# in your development session

```
. run.sh
```

you can now connect to the mongo instance on the IP given by the command  `boot2docker ip` on port 27017 (the default port).

# references

see also: 

- [https://registry.hub.docker.com/_/mongo/](https://registry.hub.docker.com/_/mongo/)
- [https://github.com/abarbanell/mongodb.docker](https://github.com/abarbanell/mongodb.docker)

