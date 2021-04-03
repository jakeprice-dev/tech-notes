---
id: 20210403212650
uuid: 95629ac4-938a-4fe4-b5b1-86704df7b2df
title: Docker Cheatsheet
date: 2021-04-03 21:26:50
modified: 
types: tech-note
categories: docker
pinned: false
tags: [docker, containers, cheatsheet]
private: false
draft: false
---

```sh
# Check Docker version:
docker version

# Pull the Ubuntu image:
docker image pull ubuntu:latest

# Check available images:
docker image ls

# Show image history:
docker history <image>

# Run container and attach to shell:
docker container run -it ubuntu:latest /bin/bash
docker container run --name ctr1 -it alpine:latest sh

# Exit container without stopping it:
Ctrl + PQ

# View container statuses:
docker container ls

# Another way to attach to the running Ubuntu container:
docker container exec -it <name/id> <process>

# Stop container (sends SIGTERM, and if process not stopped 10 secs later SIGKILL):
docker container stop <name/id>

# Remove container:
docker container rm <name/id>

# Force remove _all_ containers! Dangerous!
docker container rm $(docker container ls -aq) -f

# Show _all_ containers (stopped to):
docker container ls -a

# Build a docker image from instructions in a Dockerfile:
docker image build -t test:latest .

# Run the test image:
docker container run -d --name web1 --publish 8080:8080 test:latest

# View container logs:
docker logs <name/id>

# Run a shell (or any other command):
docker exec --interactive --tty <name/id> <command>
docker exec --interactive --tty gitea bash
docker exec --interactive --tty gitea ls

# Log onto image that won't start:
docker run -it --entrypoint /bin/bash <image-id> -s

# Build image:
docker build --build-arg VARIABLE=here --tag hello/docker:1.0 .

# Run container using image and feed in variables:
docker run -it --env-file .\.env  --entrypoint sh edcbd022ca8b

# Search DockerHub:
docker search miniflux
docker search alpine --filter "is-official=true"
docker search alpine --filter "is-automated=true"

# Port mappings:
host-port:container-port
-p 80:8080

# Inspect an image and container:
docker image inspect <image-id>
docker container inspect <container-id>
```

Keep dockerfile docker container running for debugging:

```dockerfile
ENTRYPOINT ["tail", "-f", "/dev/null"]
```

