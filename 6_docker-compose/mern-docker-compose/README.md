# Dockerizing the MERN Stack  Application: A Comprehensive Guide with Docker Compose
<br>

## To run it locally (without docker)

**1. Prerequisite**
```bash
# Linux (Ubuntu)
sudo apt install nodejs npm -y
node -v
npm -v
```

**2. Start Server**
```bash
cd mern/backend
npm install
npm start
```

**3. Start Client**
```bash
cd mern/frontend
npm install
npm run dev
```

_________________________________________________________________________________________________

## To run it using Docker only

**1. Prerequisite**
```bash
# Linux (Ubuntu)

sudo apt install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
docker --version
```

**2. Create a network for the docker containers**
```bash
docker network create mern-demo
```

**3. Build and run the client**
```bash
cd mern/frontend
docker build -t mern-frontend .
docker run --name=frontend --network=mern-demo -d -p 5173:5173 mern-frontend
docker ps
```
Open your browser at ```http://localhost:5173``` and verify.

**4. Run the mongodb container**
```bash
docker run --network=mern-demo --name mongodb -d -p 27017:27017 -v ~/opt/data:/data/db mongo:latest
```
Open your browser at ```http://localhost:27017``` and verify.

**5. Build and run the server**
```bash
cd mern/backend
docker build -t mern-backend .
docker run --name=backend --network=mern-demo -d -p 5050:5050 mern-backend
docker ps
```
Open your browser at ```http://localhost:5050``` and verify.

________________________________________________________________________________________________

## To run it using Docker Compose

**1. Prerequisite**
```bash
# Linux (Ubuntu)

sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker
docker --version
docker-compose --version
```

**2. Execute docker-compose file**
```bash
docker-compose up --build -d
```

**3. Verify:**
```bash
docker ps
docker logs backend
docker logs frontend
docker logs mongodb
```
- Backend URL: ```http://localhost:5050```.
- Frontend URL: ```http://localhost:5173```.
- Database URL: ```http://localhost:27017```.

<br>