# Docker Compose

## Docker | vs | DockerCompose

**Docker** is a containerization platform and toolset and it manages a container's lifetime (developing, shipping, and running applications in containers). 

Containers are lightweight, isolated environments that include only the necessary dependencies, libraries, and settings an application needs to run. Docker allows you to build images, start, stop, and manage individual containers with ease.

**Docker Compose** is a tool that helps define and manage multi-container Docker applications (For Example: Web applications that require a database, caching, and other microservices). 

It uses a YAML configuration file (docker-compose.yml) to specify services, networks, and volumes for all containers needed by an application, allowing you to orchestrate complex applications with just a few commands.

In practice, Docker Compose relies on Docker to run each service in its defined container, so they are typically used together. For instance, Docker will build the images, and Docker Compose will orchestrate how they work together in a single environment.

Docker Compose simplifies container management, especially for local development and testing, but for production environments, orchestration tools like Kubernetes are usually preferred due to their advanced features and scaling capabilities.


<br>


## Use Cases

### 1. Docker: Developer Environment Standardization

*Use Case:* Simplifying Setup for Developers

*Example:* A web development team working on a Django project can create a Docker image that includes Python, Django, and any other dependencies. Each developer can spin up this Docker container to work with an identical setup.

- Reduced “It works on my machine” issues: Each developer uses the same environment, so there are fewer inconsistencies across setups.
- Fast Setup: Developers can get up and running with minimal configuration.


### 2. Docker Compose: Multi-Container Development Environment

*Use Case:* Building a Full Stack Web Application

*Example:* A development team builds a web application with a React frontend, a Django backend, and a PostgreSQL database. With Docker Compose, they define all three services in a single docker-compose.yml file.

Setup in *docker-compose.yml*:

```yaml
version: '3'
services:
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
  backend:
    build: ./backend
    ports:
      - "8000:8000"
  database:
    image: postgres
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
```

- Simplified Management: With a single ```docker-compose up```, developers start the whole application.
- Service Discovery: Containers can refer to each other by name, e.g., the backend can connect to the database at database:5432.


### 3. Docker Compose: CI/CD Pipelines

*Use Case:* Automated Testing in Continuous Integration

*Example:* A project’s CI/CD pipeline (e.g., Jenkins) runs tests for a web application. The pipeline uses Docker Compose to set up a test environment, bringing up a web server and database. Tests run against this setup, and containers are torn down afterward.

- Repeatability: Tests run in a consistent environment every time.
- Cleanup: Containers can be easily removed after tests, freeing up resources.


## For Reference

```html
https://github.com/docker/awesome-compose
```

```html
https://docs.docker.com/
```

### Docker Compose | Docker Swarm | Kubernetes

**In the docker ecosystem, the closest to Kubernetes is docker Swarm**

**Docker Compose** is used to define and run multi-container Docker applications on a single host. It's great for local development/testing where you want to manage multiple containers in one setup (e.g., a web app with a database).

**Docker Swarm** is Docker’s native clustering and orchestration tool. It groups multiple Docker hosts into a single "swarm" and helps manage and schedule container deployment across nodes in the swarm. It can manage Clusters of Docker hosts with lightweight orchestration needs only.

**Kubernetes** is a comprehensive container orchestration platform designed for managing complex, distributed applications across clusters of servers. It is a large-scale, production-grade container orchestration tool.