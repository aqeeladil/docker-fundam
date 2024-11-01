# Multi Stage Docker Builds | Reduce Image Size by 800 % | Distroless Container Images


## Intro

- Multi-stage Docker builds allows you to create smaller and more efficient images by separating the build process into multiple stages.

- This method is particularly useful when building applications where you only need certain files and binaries in the final image without carrying over unnecessary dependencies.


## Why Use Multi-Stage Builds?

- Reduces image size: Only includes necessary files in the final image.

- Improves security: Excludes build tools, test dependencies, and other components from the production image.

- Streamlines CI/CD pipelines: Allows separation of build and deploy processes.


## Why Golang Example?

- The main purpose of choosing a golang based applciation to demostrate this example is, golang is a statically-typed programming language that does not require a runtime in the traditional sense. 

- Unlike dynamically-typed languages like Python, Ruby, and JavaScript, which rely on a runtime environment to execute their code, Go compiles directly to machine code, which can then be executed directly by the operating system. that's why golang apps are more secure. 

- So the real advantage of multi stage docker build and distro less images can be understand with a drastic decrease in the Image size.


## How Multi-Stage Builds Work

- In a multi-stage build, you create multiple FROM statements in a single Dockerfile. Each FROM statement starts a new stage in the build. By the final stage, only the essential files needed to run the application are copied over, leaving behind unnecessary build artifacts.

### Example-1: Multi-Stage Build for a Go Application

```html

# Stage 1: Build the application (using the golang image to compile the Go application. Set up the working directory, copies the source code, and builds the app binary)

FROM golang:1.19 AS builder
WORKDIR /app
COPY . .
RUN go mod download
RUN go build -o myapp .

# Stage 2: Create the final image with only the binary (Use a minimal alpine image. and copy only the necessary binary myapp from the builder stage. Runs the application without carrying over any unnecessary files from the build environment)

FROM alpine:latest
WORKDIR /root/
COPY --from=builder /app/myapp .
CMD ["./myapp"]

```

### Multi-Stage Builds in Complex Applications
For more complex applications, like those built with Node.js and Nginx, you can create additional stages:

```html

# Stage 1: Build the frontend application using node image

FROM node:16 AS frontend-build
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build

# Stage 2: Setup Nginx to serve the frontend (only contains the files needed to serve the application)

FROM nginx:alpine
COPY --from=frontend-build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

```

## When to Use Multi-Stage Builds

- Your build process generates artifacts that need to be deployed independently.

- You have a complex application requiring multiple build tools (e.g., Java, Node, and Nginx).

- You want to optimize image size, performance, and security.


<br><br>

## Distroless Container Images

- Distroless container images are a type of Docker image that contain only the essential parts of the operating system needed to run the application. They exclude shell utilities, package managers, and other unnecessary components, creating a minimalist, highly secure, and lightweight container environment. 

- Distroless images were introduced by Google as part of their efforts to optimize and secure containerized applications in production.

- However, for development and debugging, a full-fledged image with shell access (like Alpine or Debian) may be more convenient. You can use distroless images in production and standard images in development for flexibility.

### Security

- Distroless images are more secure than standard container images primarily because they strip out non-essential components, which reduces the attack surface and limits opportunities for exploitation.

### Example-1: Distroless Image for a Go Application

```html

# Stage 1: 
# Build Stage: Use a golang image to build the Go binary.

FROM golang:1.19 AS builder
WORKDIR /app
COPY . .
RUN go mod download
RUN go build -o myapp .

# Stage 2: 
# Create distroless image: Use the gcr.io/distroless/base image from Google’s container registry.

FROM gcr.io/distroless/base
COPY --from=builder /app/myapp /
CMD ["/myapp"]

# Result: The final image only includes the compiled binary, myapp, without any shell or extra tools.

```

### Example-2: Distroless Image for a Python Application

```html

# Use a base image with Python3

FROM gcr.io/distroless/python3
WORKDIR /app
COPY . .
CMD ["main.py"]

```

### Commonly Used Distroless Images

- gcr.io/distroless/base: Basic image without a language runtime (for statically compiled binaries like Go or C++)
- gcr.io/distroless/cc: For C/C++ applications
- gcr.io/distroless/java: For Java applications
- gcr.io/distroless/python3: For Python 3 applications
- gcr.io/distroless/nodejs: For Node.js applications

These images are available in Google’s Container Registry and can be pulled directly in your Dockerfile.

<br>


## Dockerfile execution commands

```html
git clone https://github.com/aqeeladil/docker-fundam.git
cd docker-fundam/2_multistage-docker-build/
ls

go run calculator.go

docker build -t multistage-calculator .

docker images

```

Compare the image size with & without using multistage dockerfile