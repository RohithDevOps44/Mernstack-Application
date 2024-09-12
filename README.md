# A simple MERN stack application 

### Create a network for the docker containers

`docker network create app`

### Build the client 

```sh
cd app/frontend
docker build -t frontend .
```

### Run the client

`docker run --name=frontend --network=app -d -p 5173:5173 frontend`

### Verify the client is running

Open your browser and type `http://localhost:5173`

### Run the mongodb container

`docker run --network=app --name mongodb -d -p 27017:27017 -v ~/opt/data:/data/db mongo:latest`

### Build the server

```sh
cd app/backend
docker build -t backend .
```

### Run the server

`docker run --name=backend --network=app -d -p 5050:5050 backend`

## Using Docker Compose

`docker compose up -d`

