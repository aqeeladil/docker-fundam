# Docker-Hello-World

## 1. Docker Installation

```bash
# Install Docker
sudo apt update
sudo apt install docker.io -y

# Start docker daemon
sudo systemctl start docker

# To grant access to your user to run the docker command, you should add the user to the Docker Linux group. Docker group is create by default when docker is installed.
sudo usermod -aG docker ubuntu

# In the above command `ubuntu` is the name of the user, you can change the username appropriately.

# You need to logout and login back for the changes to be reflected.

# Verify that docker is up and running.
docker run hello-world
```

## 2. Clone the repository

```bash
git clone https://github.com/aqeeladil/docker-fundam.git
cd docker-fundam/1_hello-world/
```

## 3. Build, run and push docker image

```bash
# Create an account with `https://hub.docker.com/` and login
docker login   

# Build your first Docker Image
docker build -t aqeeladil/my-hello-world-app-image:latest .

# Verify
docker images

# Run the docker container
docker run -it aqeeladil/my-hello-world-app-image
# Output: Hello World

# Push the Image to DockerHub
docker push aqeeladil/my-hello-world-app-image
```


