---
theme: seriph
background: https://images.unsplash.com/photo-1491002052546-bf38f186af56?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1208&q=80
fonts:
    sans: 'Roboto'
    serif: 'Roboto'
    mono: 'JetBrains Mono'

---

# Docker

siriuskoan

---

# Outline
- Introduction
- Container v.s VM
- How It Works
- Preparation
- Starting to Use Docker
  - `Dockerfile`
  - `docker-compose`

---

# Introduction

Docker is an open platform for developing, shipping, and running applications.

Docker enables developers to separate the applications from the infrastructure and setup a clean environment for it.

![](/docker-icon.png)

<!--

From https://zh.wikipedia.org/wiki/File:Docker_(container_engine)_logo.svg

-->

---

# Container v.s VM

Docker use a technology called "container" to separate the applications from the infrastructure.

Besides container, another technology called virtual machine (VM) can also accomplish this.

The differences are that VM fully create an independent machine with independent OS, library, etc., and container creates a "container" based on OS of infrastructure server.

Therefore
- Container is faster and more lightweight than VM.
- VM is safer than container.
- Container is more reusable than VM.

<!--

Container is more reusable since it uses image, we will talk about later.

-->

---

# Container v.s VM

![](/containers-vs-virtual-machines.jpg)

<!--

From https://www.weave.works/blog/a-practical-guide-to-choosing-between-docker-containers-and-vms

-->

---

# How It Works

We need an "image" to create a container.

An image is a template containing the library, config, etc. for the applications.

Images are in "repository", and [Docker Hub](https://hub.docker.com/) plays the role similar to GitHub.

Developers can develop their own images and push them to Docker Hub, and they can also use others images.

For example, if we want to build a load balancer with Nginx, we can use [Nginx image](https://hub.docker.com/_/nginx).

Every image has a name, besides, 

---

# Preparation

Before starting using Docker, we have to do some preparation first.

- Install it.

  `$ sudo apt install docker.io`

- Check whether Docker is running.

  `$ sudo service docker status`

---

# Starting to Use Docker

After installation, we can now use Docker to create container.

- Run the first Docker container with `hello-world` image.

  `$ sudo docker run hello-world`

- Check container status.

  `$ sudo docker ps -a`

- Remove the container.

  `$ sudo docker rm [container name or container id]`

<!--

- `docker run` command will check whether the image is in local storage. If not, it will fetch the image from Docker Hub.

  It will automatically give the contain a name and print its id.

  Unable to find image 'hello-world:latest' locally` means it cannot find it locally, and `latest: Pulling from library/hello-world` means it fetches it from remote repo.

- `-a` will show all containers, while no `-a` will only show active ones.

-->

---

# Starting to Use Docker

- Check the logs.

  `$ sudo docker logs [container name or container id]`

- Show the images stored in local repository.

  `$ sudo docker images ls`

<!--

- You can see `hello-world` image with tag `latest`.

[hello-world image](https://hub.docker.com/_/hello-world)

-->

---

# Starting to Use Docker

`docker run [OPTIONS] IMAGE [COMMAND] [ARGS]`

Options
- `-d` or `--detach` - Run container in background and print container ID
- `--name` - Set container's name
- `--rm` - Automatically remove the container after it exits
- `-p [IP]:[CONTAINER PORT]:[HOST PORT]/[PROTOCOL]` - Publish a container's port(s) to the host, for example, `127.0.0.1:80:8080/tcp` expose `80` port on container to `8080` port on host
- `-it` - Allocate a pseudo-TTY connected to the container’s stdin
- `--read-only` - Mount the container's root filesystem as read only

<!--

Let's check out some `docker run` usage.

`-i` and `-t` are two different options, and we can enter the container and run some commands by combining them.

-->

---

# Starting to Use Docker

We can also give some commands to container by using `docker run`.

For example

- `$ sudo docker run ubuntu bash` can create an container with ubuntu image and start a bash session.
- `$ sudo docker run ubuntu touch /root/test.txt` can create a file called `test.txt` after the container is created.

---

# Starting to Use Docker

`docker exec` can run a command in a running container.

1. `$ sudo docker run --name my_ubuntu -d -it ubuntu` - Create an ubuntu container.
2. `$ sudo docker exec -it my_ubuntu bash` - Enter the ubuntu container with bash.

<!--

Notice the `-it`.

Demo the whole section.

-->

---

# Starting to Use Docker - `Dockerfile`

Sometimes the image itself is too simple, we may need to enter the container, run some commands and configure the server to the way we want.

However, it is error-prone since it is done by human.

We can use `Dockerfile` to complete the work automatically.

`Dockerfile` specifies the instructions to build a container, and Docker will automatically follow the instructions and configure the container as we want.

---

# Starting to Use Docker - `Dockerfile`

Let's see an example, and we will explain the example line by line.

```dockerfile
FROM python:3.9.5
WORKDIR /code
ENV ENVIRONEMENT="PRODUCTION"
COPY . /code
RUN chmod +x /code/docker-entrypoint.sh \
    && apt update \
    && apt upgrade -y \
    && apt install -y --no-install-recommends gcc default-mysql-client vim \
    && apt clean \
    && python3 -m pip install -r requirements.txt \
    && python3 -m pip cache purge
CMD /usr/local/bin/python3 -m gunicorn -b 0.0.0.0:5000 manage:app
EXPOSE 5000
```

---

# Starting to Use Docker - `Dockerfile`

- `FROM` - Sets the base image for subsequent instructions
- `WORKDIR` - Sets the working directory
- `ENV` - Sets environment variables
- `COPY` - Copies local files to container
- `RUN` - Runs specified commands
- `CMD` - Provides defaults for an executing container, and there can be only one `CMD` instruction
- `EXPOSE` - Informs Docker that the container listens on the specified network ports at runtime.

<!--

`COPY` means the data in container and in host are independent

-->

---

# Starting to Use Docker - `Dockerfile`

After finishing `Dockerfile`, we can build it as an image.

- `$ sudo docker build .` - Builds `Dockerfile` in current directory as an image
- `$ sudo docker build -t my_docker_image:test .` - Builds `Dockerfile` with name `my_docker_image` and tag `test`

---

# Starting to Use Docker - `docker-compose`

Sometimes we need to connect multiple containers and make them work together.

`docker-compose` is a tool for defining and running multi-container Docker applications.

The services are defined in a YAML file named `docker-compose.yaml`.

![](/docker-compose-logo.png)

---
layout: two-cols
---

# Starting to Use Docker - `docker-compose`

::left::

```yaml
version: '2'

services:
  web:
    build: ./src
    container_name: web
    depends_on:
      - db
    restart: on-failure
    environment:
      - ENV=production
    volumes:
      - ./data/log:/code/log
    networks:
      - net
```

::right::

```yaml
# services:
  nginx:
    image: nginx
    container_name: nginx
    depends_on:
	  - web
    restart: on-failure
    ports:
      - 80:80
      - 443:443
    volumes:
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/conf.d:/etc/nginx/conf.d
    network:
      - net

networks:
  net:
```

<!--

There are two services - web and nginx, and they are connected to the same network `net`.

-->

---

# Starting to Use Docker - `docker-compose`

- `build` - Manifests the image should be built with `Dockerfile` under specified directory
- `container_name` - Sets the container name
- `depends_on` - Starts the container after the containers on the list start
- `restart` - Restarts on specified situations. The situations can be: `no`, `on-failure`, `always` and `unless-stopped`
- `environment` - Sets environment variables
- `ports` - Sets the port mapping
- `volumes` - Mount directories on host to container
- `networks` - Connects to specified networks

<!--

Talk more about `volumes`.

-->

---

# Starting to Use Docker - `docker-compose`

- After creating `docker-compose.yaml`, we can launch the containers. We can also add `-d` options to make the containers run in the background.

  `$ sudo docker-compose up -d`

  For the example above, we can also add `--build` or `--no-build` to force rebuilding or no rebuilding.

- Check whether they are working correctly.

  `$ sudo docker-compose ps`

- Stops containers and removes containers, networks, volumes, and images created by up.

  `$ sudo docker-compose down`

  For the example above, we can also add `--rmi` to remove images after shutdown.

