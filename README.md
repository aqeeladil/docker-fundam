# Containers | Docker

- **Problem Statement :**

    - Developer: It works on my machine.

    - Production Team: It is not working properly in the production environment.

## Intro to Containers

- A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another. 

- A Docker container image is a lightweight, standalone, executable package of software that includes everything needed to run an application (code, runtime, system tools, system libraries and settings).

- As hypervisors were an advancement to physical servers, containers are an advancement to hypervisor. They only take resources from a system that are required to deploy an app onto a server.

- Containers don't have a complete OS. They run on host OS and use the resources from the host OS. But if a container is not running, it don't use resources from kernel (host os). 

- Although docker containers use system resources from host os (kernel), but each container is logically isolated from other conatiners.

- A docker image is a blueprint, with the help of which a docker container is made. To create a docker image for a Flask app. I need resources like Ubuntu, apache, packages, dependencies, etc in that image.

![Screenshot 2023-02-07 at 7 18 10 PM](https://user-images.githubusercontent.com/43399466/217262726-7cabcb5b-074d-45cc-950e-84f7119e7162.png)

## Containers vs Virtual Machine 

Containers and virtual machines are both technologies used to isolate applications and their dependencies, but they have some key differences:

1. **Resource Utilization:** Containers share the host operating system kernel, making them lighter and faster than VMs. VMs have a full-fledged OS and hypervisor, making them more resource-intensive.

2. **Portability:** Containers are designed to be portable and can run on any system with a compatible host operating system. VMs are less portable as they need a compatible hypervisor to run.

3. **Security:** VMs provide a higher level of security as each VM has its own operating system and can be isolated from the host and other VMs. Containers provide less isolation, as they share the host operating system.

4.  **Management:** Managing containers is typically easier than managing VMs, as containers are designed to be lightweight and fast-moving.

## Why are containers light weight ?

- Containers are lightweight because they use a technology called containerization, which allows them to share the host operating system's kernel and libraries, while still providing isolation for the application and its dependencies. This results in a smaller footprint compared to traditional virtual machines, as the containers do not need to include a full operating system. 

- Additionally, Docker containers are designed to be minimal, only including what is necessary for the application to run, further reducing their size.

- For example, to run a Java application, we need:

   - the application, 
   - its run-time dependencies
   - & some system dependencies that are mandatory to run the application.

- **Let's try to understand this with an example:**

    - Below is the screenshot of official ubuntu base image which you can use for your container. It's just ~ 22 MB, isn't it very small ? on a contrary if you look at official ubuntu VM image it will be close to ~ 2.3 GB. So the container base image is almost 100 times less than VM image.

![Screenshot 2023-02-08 at 3 12 38 PM](https://user-images.githubusercontent.com/43399466/217493284-85411ae0-b283-4475-9729-6b082e35fc7d.png)

- **Files and Folders in containers base images:**

    - `/bin:` contains binary executable files, such as the ls, cp, and ps commands.

    - `/sbin:` contains system binary executable files, such as the init and shutdown commands.

    - `/etc:` contains configuration files for various system services.

    - `/lib:` contains library files that are used by the binary executables.

    - `/usr:` contains user-related files and utilities, such as applications, libraries, and documentation.  

    - `/var:` contains variable data, such as log files, spool files, and temporary files. 

    - `/root:` is the home directory of the root user.

- **Files and Folders that containers use from host operating system:**

    - Docker containers can access the `host file system` using bind mounts, which allow the containers to read and write files in the host file system.

    - The `host's networking stack` is used to provide network connectivity to the container. Docker containers can be connected to the host's network directly or through a virtual network.

    - The host's kernel handles `system calls` from the container, which is how the container accesses the    host'sresources, such as CPU, memory, and I/O.

    - Docker containers use linux `namespaces` to create isolated environments for the container's processes.Namespaces provide isolation for resources such as the file system, process ID, and network.

    - Docker containers use `cgroups` (control groups) to limit and control the amount of resources, such as cpu, memory, and I/O, that a container can access.

- It's important to note that while a container uses resources from the host operating system, it is still isolated from the host and other containers, so changes to the container do not affect the host or other containers.

## Docker Architecture

- Docker is very much dependent on Docker engine (daemon) to execute docker files, build docker image & finally run a docker container.

- Docker Engine is a single point of failure. If it is down, all your docker containers will stop working.

- `BUILDAH` solves this problem of Docker Engine. In buildah tool you don't have to write docker files. You will simply add buildah commands in a shell script, and it will create a docker image for you.

![image](https://user-images.githubusercontent.com/43399466/217507877-212d3a60-143a-4a1d-ab79-4bb615cb4622.png)

- **Important Commands:**

    - `docker build` -> builds docker images from Dockerfile (which base image to choose? what dependencies should be installed   for the application to run? etc.).

    - `docker run` -> runs container from docker .

    - `docker push` -> push the container image to public/private regestries (like docker hub or ECR) to share the docker images.

![Screenshot 2023-02-08 at 4 32 13 PM](https://user-images.githubusercontent.com/43399466/217511949-81f897b2-70ee-41d1-b229-38d0572c54c7.png)

## Important Terms

- **Docker Daemon:**

    - The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.

- **Docker Client**

    - The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.

- **Docker Desktop**

    - Docker Desktop is an easy-to-install application for your Mac, Windows or Linux environment that enables you to build and share containerized applications and microservices. Docker Desktop includes the Docker daemon (dockerd), the Docker client (docker), Docker Compose, Docker Content Trust, Kubernetes, and Credential Helper. For more information, see Docker Desktop.

- **Docker Registries**

    - A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

    - When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry.

- **Dockerfile**

    - Dockerfile is a file where you provide the steps to build your Docker Image. 

- **Images**

    - An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

    - You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

- Docker ADD | Docker COPY

    - Docker ADD can copy the files from a URL, while Docker COPY can only copy files from host system into the container.

- CMD | Entrypoint

    - Entrypoint is more powerfull.

    - CLI arguments using the Docker run command will override the arguments specified using CMD instruction. 

    - Whereas Entrypoint instruction in the shell form will override the additional arguments provided using CLI parameters or even through the CMD command. 

## Real-Time Challenges with Docker

### 1. Single Daemon Process (Single Point of Failure)

- **Issue:** Docker relies on a single daemon process. If it crashes, all running containers go down.

- **Solution:** Use **Podman**, which is daemonless, or Kubernetes for orchestrating containers across multiple nodes to ensure high availability.

### 2. Security Risks due to Docker Daemon Running as Root

- **Issue:** The Docker daemon runs with root privileges, posing a significant security risk if compromised.

- **Solution:** Use **Rootless Docker**, **Podman**, or tools like **gVisor** and **Kata Containers** to provide an extra security layer.

### 3. Resource Constraints (Too Many Containers on a Single Host)

- **Issue:** Running excessive containers on a single host can cause performance degradation, CPU throttling, and memory exhaustion.

- **Solution:** Implement **resource limits** using `--memory`, `--cpu-shares`, or `cgroups`, and scale containers across multiple nodes using **Kubernetes**.

### 4. Networking Issues

- **Issue:** Containers communicate through virtual networks, which may introduce latency, DNS resolution issues, or complex networking configurations.

- **Solution:** Use **Docker Compose** for managing multi-container applications and **Overlay Networks** in **Swarm or Kubernetes** for better communication.

### 5. Storage Management & Data Persistence

- **Issue:** Docker containers are ephemeral; if a container crashes, its data is lost unless properly managed.

- **Solution:** Use **Docker Volumes** or **Bind Mounts** to persist data and manage it efficiently.

### 6. Logging and Monitoring Challenges

- **Issue:** Tracking logs and monitoring container performance can be complex, especially in large-scale deployments.

- **Solution:** Use centralized logging tools like **ELK (Elasticsearch, Logstash, Kibana)**, **Prometheus**, or **Grafana** for better observability.

### 7. Image Bloat and Vulnerabilities

- **Issue:** Large Docker images increase build times, deployment times, and storage requirements. Additionally, outdated images may contain security vulnerabilities.

- **Solution:** Use **minimal base images** like Alpine, scan images using **Trivy**, and clean up unnecessary layers.

## Steps to secure a container

- Use distroless images with not too many packages as your final image in multi-stage-build, so that there is less chance of CVE or security issues.

- Ensure that the networking is configured properly. If required, configure custom bridge networks and assign them to isolate the containers.

- Use utilities like Sync to scan your container images.

## Stop Writing Dockerfiles - Use Docker Init

- Initialize a project with the files necessary to run the project in a container.

- Run `docker init` in your project directory to be walked through the creation of the following files with sensible defaults for your project:

    ```
    .dockerignore
    Dockerfile
    compose.yaml
    README.Docker.md
    ```

- If any of the files already exist, a prompt appears and provides a warning as well as giving you the option to overwrite all the files. If `docker-compose.yaml` already exists instead of `compose.yaml`, `docker init` can overwrite it, using `docker-compose.yaml` as the name for the compose file.
