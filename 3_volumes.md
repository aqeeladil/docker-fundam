# Docker Volumes

## Problem Statement

It is a very common requirement to persist the data in a Docker container beyond the lifetime of the container. However, the file system of a Docker container is deleted/removed when the container dies.

## Solution

There are 2 different ways how docker solves this problem.

1. Volumes
2. Bind Directory on a host as a Mount

### Volumes 

Volumes aims to solve this problem by providing a way to store data on the host file system, separate from the container's file system, 
so that the data can persist even if the container is deleted and recreated.

Docker volumes are the preferred way to manage persistent data in Docker. They are stored in Docker’s storage directory (e.g., /var/lib/docker/volumes/ on Linux) and are managed directly by Docker, which helps keep the host filesystem isolated.

![image](https://user-images.githubusercontent.com/43399466/218018334-286d8949-d155-4d55-80bc-24827b02f9b1.png)


Volumes can be created and managed using the docker volume command. You can create a new volume using the following command:

```html
docker volume create <volume_name>
```

```html
# Removing a Volume: 
docker volume rm my_volume

# List volumes:
docker volume ls

# Check the details of a volume:
docker volume inspect <volume_name>

# Check the details of a conatiner:
docker inspect <container_name>

# List running containers
docker ps
```

Once a volume is created, you can mount it to a container using the -v or --mount option when running a docker run command. 

For example:

```html
docker run -it -v <volume_name>:/data <image_name> /bin/bash

OR

docker run -d --mount source=my_volume, target=/my_app my_image:latest
```

This command will mount the volume <volume_name> to the /data directory in the container. Any data written to the /data directory
inside the container will be persisted in the volume on the host file system.

### Bind Directory on a host as a Mount

Bind mounts link a specific directory on the host filesystem to a directory in the container. This method is more flexible than volumes but tightly couples the container to the host machine's directory structure, which can make migration or scaling challenging.

For example, 

```html
docker run -it -v /host/path:/container/path <image_name> /bin/bash
```

This command mounts /host/path from the host to /container/path inside the container.


###########################################################################

## Key Differences between Volumes and Bind Directory on a host as a Mount

Volumes are managed, created, mounted and deleted using the Docker API. However, Volumes are more flexible than bind mounts, as they can be managed and backed up separately from the host file system, and can be moved/shared between multiple containers and hosts, making them useful for applications that need shared storage.

Since bind mounts reference the host’s filesystem paths, they can create dependencies that limit portability across different host environments.

Docker volumes generally offer better performance on Docker-native systems than bind mounts, especially when managed by Docker’s storage drivers.

Docker volumes are generally the better choice for production environments because of their management and performance benefits, 

While bind mounts are more suitable for development scenarios where direct access to host files is required or in cases where specific host directories are necessary (like configuration files or log directories).


