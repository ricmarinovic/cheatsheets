---
title: Docker
category: Tools
tags: [WIP]
updated: 2020-12-30
weight: -1
---

Intro
-------------------------------------

Running a container from an image:

```shell
docker run <image name> <command>
```

where `<command>` optionally overrides the default command defined on the image.

#### Options

- `-p`, `--publish list` publish container’s ports to the host.
    - format: `local_port:container_port`, e.g. `8080:8080`.
- `--name` assign a name to the container.
- `--rm` automatically remove the container when it exits.
- `-d`, `--detach` Run container in background and print container ID.
- `-v`, `--volume list` bind mount a volume.
    - format: `local_path:container_path`.
    - If `local_path` is not provided, it will not be mapped.
