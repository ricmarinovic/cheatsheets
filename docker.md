---
title: Docker
category: Tools
tags: [WIP]
prism_languages: [docker]
updated: 2021-02-17
weight: -1
---

Intro
-------------------------------------

### Lists running containers

```shell
docker ps
```

#### Options

- `-a,` `--``all` lists all containers (even those not currently running).
- `-l,` `--``latest` show the latest created container.

### Running a container from an image

```shell
docker run <image name> <command>
```

`<command>` optionally overrides the default command defined on the image.

#### Options

- `-p`, `--publish list` publish container’s ports to the host.
    - format: `local_port:container_port`, e.g. `8080:8080`.
- `--name` assign a name to the container.
- `--rm` automatically remove the container when it exits.
- `-d`, `--detach` Run container in background and print container ID.
- `-v`, `--volume list` bind mount a volume.
    - format: `local_path:container_path`.
    - If `local_path` is not provided, it will not be mapped.

Docker `run` is a combination of `create` and `start`.

```shell
# Returns a `pid`
docker create <image name>
```

```shell
docker start <pid>
```

#### Options

- `-a,` `--``attach` shows outputs from container.

### Execute command in a running container

```shell
docker exec -it <pid> <command>
```

#### Options

- `-i,` `--``interactive` keep Input open.
- `-t,` `--``tty` allocate a pseudo-TTY.

### Get logs from a container

```shell
docker logs <pid>
```

### Stop containers

Send SIGTERM to process:

    docker stop <pid>

Send SIGKILL to process:

    docker kill <pid>

Remove a docker container with conflicting names

    docker rm <container name>

### Delete containers

```shell
docker system prune
```

Remove all stopped containers, dangling images, build cache and networks not used by at least one container:

Returns the `pid` of deleted containers and shows total reclaimed space.


Create a Dockerfile
-------------------------------------

On a file simply named `Dockerfile`:

```dockerfile
# Use an existing docker image as a base
FROM alpine
# Donwload and install a dependency
RUN apk add --update redis
# Tell the image what to do when it starts as a container
CMD ["redis-server"]
```

### Build a docker image from the Dockerfile

```shell
docker build <directory>
```

Returns a `pid`.

#### Options

- `-t,` `--``tag list` Name `list` in the format `name:tag`.
    - `<docker id>/<project name>:<version>`.
    - Example: `docker build -t stephengrider/redis:latest redis-server`.
    - Then run with `docker run stephengrider/redis`.
- `-f,` `--``file name` Name of the Dockerfile (default is `PATH/Dockerfile`).

### List all docker images on the machine

    docker image ls

### Run the docker image

    docker run <pid>

### Create an image from a running container example

```shell
docker run -it alpine sh
docker ps # check the pid for the running container
docker commit -c 'CMD ["redis-server"]' <pid>
docker run <pid>
```

### Dockerfile commands

Copy files from local to container:

```
COPY <path relative to build context> <path on container>
```

Any following comand will be executed relative to this path in the container:

```
WORKDIR <path>

EXPOSE <port number>

VOLUME <container path>
```


Docker Compose
-------------------------------------

On a file named `docker-compose.yml`:

```yaml
version: '3'
services:
    web:
    container_name: web
    build:
        context: .
        dockerfile: Dockerfile.dev
    restart: on-failure
    ports:
        - "3000:3000"
    volumes:
        - /app/node_modules
        - .:/app
        command: ["yarn", "test"]
```

### Docker Compose commands

Run all services:

```shell
docker-compose up
```

#### Options

- `--build` build images before starting containers.
- `-d, -detach` Run containers in background, print new container names.

Stop and remove containers, networks, images, and volumes:

```shell
docker-compose down
```

### Restart policies

- `"``no``"`
- `always`
- `on-failure` only restart if the container exists with an error code.
- `unless-stopped` always restart unless forced with `docker stop`.
