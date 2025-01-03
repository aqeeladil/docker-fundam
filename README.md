# Containers

## Problem Statement :
- Developer: It works on my machine.
- Production Team: It is not working properly in the production environment.

### Intro to Containers
- Containers are an advancement to virtual machines.
- Physical servers vs Vms vs Containers
- Container only take resources from a system that are required to deploy an app onto a server. 
- While hypervisors take OS-level resources from the system. This means VMs consume OS-level resources which is a costly option. While docker consumes fewer resources and is lighter in weight. 
- Most of times Vm's/Ec2 instances waste lot of resources. 
- Vm's have a complete operating system. so they are heavy in size and use more resources . Due to this they are more secure than containers. 
- A container is a base image/ package which is a combination of  application + application libraries + system libraries. 
- Containers don't have a complete OS. they run on host OS and use the resources from the host OS. But if a container is not running, it don't use resources from kernel (host os). 
- Although docker containers use system resources from host os (kernel), but each container is logically isolated from other conatiners.
- Containers are light weight & easy to ship/move. 
- To create a docker image for a python_flask app. I need resources like Ubuntu, apache, packages, dependencies, etc in that image.
- A docker image is a blueprint, with the help of which a docker container is made.

## What is a container ?

A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application: code, runtime, system tools, system libraries and settings.

A container is a bundle of Application, Application libraries required to run your application and the minimum system dependencies.

![Screenshot 2023-02-07 at 7 18 10 PM](https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png)



## Containers vs Virtual Machine 

Containers and virtual machines are both technologies used to isolate applications and their dependencies, but they have some key differences:

    1. Resource Utilization: Containers share the host operating system kernel, making them lighter and faster than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.

    2. Portability: Containers are designed to be portable and can run on any system with a compatible host operating system. VMs are less portable as they need a compatible hypervisor to run.

    3. Security: VMs provide a higher level of security as each VM has its own operating system and can be isolated from the host and other VMs. Containers provide less isolation, as they share the host operating system.

    4.  Management: Managing containers is typically easier than managing VMs, as containers are designed to be lightweight and fast-moving.



## Why are containers light weight ?

Containers are lightweight because they use a technology called containerization, which allows them to share the host operating system's kernel and libraries, while still providing isolation for the application and its dependencies. This results in a smaller footprint compared to traditional virtual machines, as the containers do not need to include a full operating system. Additionally, Docker containers are designed to be minimal, only including what is necessary for the application to run, further reducing their size.

- Containers do not have a complete OS. They follow the concept of shared libraries. 
- They have very minimal system dependencies that are required to run your application. 
- For example, to run a Java application, we need:
   - the application, 
   - its run-time dependencies
   - & some system dependencies that are mandatory to run the application.


Let's try to understand this with an example:

Below is the screenshot of official ubuntu base image which you can use for your container. It's just ~ 22 MB, isn't it very small ? on a contrary if you look at official ubuntu VM image it will be close to ~ 2.3 GB. So the container base image is almost 100 times less than VM image.

![Screenshot 2023-02-08 at 3 12 38 PM](https://user-images.githubusercontent.com/43399466/217493284-85411ae0-b283-4475-9729-6b082e35fc7d.png)


To provide a better picture of files and folders that containers base images have and files and folders that containers use from host operating system (not 100 percent accurate -> varies from base image to base image). Refer below.



### Files and Folders in containers base images

```
    /bin: contains binary executable files, such as the ls, cp, and ps commands.

    /sbin: contains system binary executable files, such as the init and shutdown commands.

    /etc: contains configuration files for various system services.

    /lib: contains library files that are used by the binary executables.

    /usr: contains user-related files and utilities, such as applications, libraries, and documentation.

    /var: contains variable data, such as log files, spool files, and temporary files.

    /root: is the home directory of the root user.
```



### Files and Folders that containers use from host operating system

```
    The host's file system: Docker containers can access the host file system using bind mounts, which allow the container to read and write files in the host file system.

    Networking stack: The host's networking stack is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

    System calls: The host's kernel handles system calls from the container, which is how the container accesses the host's resources, such as CPU, memory, and I/O.

    Namespaces: Docker containers use Linux namespaces to create isolated environments for the container's processes. Namespaces provide isolation for resources such as the file system, process ID, and network.

    Control groups (cgroups): Docker containers use cgroups to limit and control the amount of resources, such as CPU, memory, and I/O, that a container can access.
    
```

It's important to note that while a container uses resources from the host operating system, it is still isolated from the host and other containers, so changes to the container do not affect the host or other containers.

**Note:** There are multiple ways to reduce your VM image size as well, but I am just talking about the default for easy comparision and understanding.

so, in a nutshell, container base images are typically smaller compared to VM images because they are designed to be minimalist and only contain the necessary components for running a specific application or service. VMs, on the other hand, emulate an entire operating system, including all its libraries, utilities, and system files, resulting in a much larger size. 

I hope it is now very clear why containers are light weight in nature.



## Docker

- Docker is an open-source containerization platform.
- It is used to manage the lifecycle of a container.
- It enables developers to package applications into containers.
- I use docker to build docker images, wtite docker files, run docker containers, push them to registeries, etc.
- Docker allows you to have an absolutely sealed, air-tight container. And these containers are the absolute heart of docker.
- These containers wrap up your entire code and these are absolutely portable. This means where ever you put these containers it will work exactly the same as it works on your local machine.
- Docker also allows you to have social containers. This means you can publish these containers onto a social platform (Docker-Hub).
- It's a client-side application program. You can put in it whatever you want and move anywhere.
- It also acts like a service and can be deployed onto any server. This means you can take your container and deploy it on any server.
- It also acts as a social networking platform (Docker-Hub) where you can share your docker image.
- Docker is very much dependent on Docker engine (daemon) to execute docker files, build docker image & finally run a docker container.
- Docker Engine is a single point of failure. If it is down, all your docker containers will stop working.
- BUILDAH solves this problem of Docker Engine. In buildah tool you don't have to write docker files. You will simply add buildah commands in a shell script, and it will create a docker image for you.


### What is Docker ?

Docker is a containerization platform that provides easy way to containerize your applications, which means, using Docker you can build container images, run the images to create containers and also push these containers to container regestries such as DockerHub, Quay.io and so on.

In simple words, you can understand as `containerization is a concept or technology` and `Docker Implements Containerization`.


### Docker Architecture ?

![image](https://user-images.githubusercontent.com/43399466/217507877-212d3a60-143a-4a1d-ab79-4bb615cb4622.png)

The above picture, clearly indicates that Docker Deamon is brain of Docker. If Docker Deamon is killed, stops working for some reasons, Docker is brain dead :p (sarcasm intended).

### Docker LifeCycle 

We can use the above Image as reference to understand the lifecycle of Docker.

There are three important things,

1. docker build -> builds docker images from Dockerfile (which base image to choose? what dependencies should be installed for the application to run? etc.)
2. docker run   -> runs container from docker images
3. docker push  -> push the container image to public/private regestries to share the docker images.

![Screenshot 2023-02-08 at 4 32 13 PM](https://user-images.githubusercontent.com/43399466/217511949-81f897b2-70ee-41d1-b229-38d0572c54c7.png)



### Understanding the terminology (Inspired from Docker Docs)


#### Docker daemon

The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.


#### Docker client

The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.


#### Docker Desktop

Docker Desktop is an easy-to-install application for your Mac, Windows or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see Docker Desktop.


#### Docker registries

A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry.
Docker objects

When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.


#### Dockerfile

Dockerfile is a file where you provide the steps to build your Docker Image. 


#### Images

An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.


### Docker ADD | vs | Docker COPY

- Docker ADD can copy the files from a URL, while Docker COPY can only copy files from host system into the container.


### CMD | vs | Entrypoint

- Entrypoint is more powerfull.
- CLI arguments using the Docker run command will override the arguments specified using CMD instruction. 
- Whereas Entrypoint instruction in the shell form will override the additional arguments provided using CLI parameters or even through the CMD command. 


### Problem: Real Time Challenges with Docker

- Docker is a single daemon process. Which can cause a single point of failure. If the docker daemon goes down for some reason all the applications are down.
- Docker Daemon runs as a root user. Which is a security threat. Any process running as a root can have adverse effects. When it is compromised for security reasons, it can impact other applications or containers on the host.
- Solution: This problem is solved by Podman

- If you are running too many containers on a single host, you may face issues with resource constraints. This can result in slow performance or crashes.


### Steps to secure a container

1. Use distroless images with not too many packages as your final image in multi-stage-build, so that there is less chance of CVE or security issues.
2. Ensure that the networking is configured properly. If required, configure custom bridge networks and assign them to isolate the containers.
3. Use utilities like Sync to scan your container images.


### Most Useful Docker Command
### Stop Writing Dockerfiles - Use Docker Init

Initialize a project with the files necessary to run the project in a container.
Docker Desktop provides the docker init CLI command. Run docker init in your project directory to be walked through the creation of the following files with sensible defaults for your project:

```html
.dockerignore
Dockerfile
compose.yaml
README.Docker.md
```

If any of the files already exist, a prompt appears and provides a warning as well as giving you the option to overwrite all the files. If docker-compose.yaml already exists instead of compose.yaml, docker init can overwrite it, using docker-compose.yaml as the name for the Compose file.
