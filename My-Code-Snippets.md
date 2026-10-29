# 📌📌 Docker

```
# helping commands
docker --version
docker info
docker help
docker container --help

# pulling docker images
docker pull python:3.9-slim

# checking 
docker images
docker ps
docker ps -a

# inspect container, shows detailed container configuration 
# including network settings, mounts and environment variables 
docker inspect ml-dev

# Create and run a container that executes a simple Python ML library check and then exit 
docker run python:3.9-slim python -c "import sys; print(f'Python {sys.version}')"

# creating container and getting inside it, after exit this container will stop 
docker run -it --name ml-dev python:3.9-slim /bin/bash

# start existing container again, -ai flag is used to interact with it, after exit this container runs on background
docker start -ai ml-dev

# inside into running container
docker exec -it ml-dev /bin/bash

# remove container
docker rm ml-dev

# remove image with force
docker rmi -f python:3.9-slim
```

### ✅✅ Creating simple Dockerfile to build a image
```
FROM python:3.12-slim
WORKDIR /app

# Copy requirements first for better caching
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application files
COPY app.py .
COPY model.pkl .

# Expose port
EXPOSE 5000

# Run the application
CMD ["python", "app.py"]
```

```
# Building docker image from docker file, -t stands for tag, . means current directory as source of files for build
docker build -t ml-serving-api:v1 .
docker images | grep ml-serving

# running Container from image
# ports: "8000:5000"
# This maps host port 8000 to container port 5000
# Meaning: if visit http://localhost:8000 on machine, actually connecting to inside container
# First number = host port, second number = container port
docker run --name ml-api \
    -p 8000:5000 \
    -d ml-serving-api:v1

# view logs
docker logs ml-api

# check endpoint based on endpoint name 
curl http://localhost:8000/health

# stop container
docker stop ml-api

# run container with different restart policies 
docker run -d --name ml-inference-api --restart=always python:3.11-slim
docker run -d --name ml-monitor-unless --restart=unless-stopped python:3.11-slim
docker run -d --name ml-training-job --restart=on-failure python:3.11-slim

# Check health inside container
docker exec ml-inference-api curl -s http://localhost:5000/health

# Check Restart Count
docker inspect ml-inference-api --format '{{.RestartCount}}'

# Restart Docker Daemon, This command restarts Docker service (daemon) on a Linux system using systemd
sudo systemctl restart docker

# Check Container Status using filter
docker ps -a --filter name=ml-training-job

# Remove all dangling images
# A dangling image is an image that is no longer tagged and appears in listings as <none>:<none>
docker image prune

# removes all unused images not used by running containers
docker image prune -a

# removes unused images, containers, networks and cache but still won't remove images currently used by running containers 
docker system prune -a

# to list images from a specific repository, use reference filter
docker images --filter reference="nginx"

# To list images created before a specific image use before 
docker images --filter before=alpine 

# To see all available image filters
docker images --help

# Adding Labels to Images
# Example Dockerfile
FROM alpine:latest
LABEL com.example.version="1.0"
COPY . /app
CMD ["sh", "/app/start.sh"]

# To build this image and tag it as myapp:v1.0
docker build -t myapp:v1.0 .

# verify image
docker images --filter label=com.example.version=1.0

# To list images with the latest tag
docker images --filter reference="*:latest"

# --format flag allows us to customize output 
docker images --format "{{.Repository}}: {{.Tag}}: {{.Size}}"

# search images in docker hub 
docker search alpine
docker search alpine --filter "is-official=true"
docker search alpine --limit 50

# An image digest is a cryptographic hash (typically SHA256) that uniquely identifies an image based on its content
# Unlike tags, which can change, a digest is immutable
docker images --digests alpine

# pull image using digest instead of tag
docker pull alpine@sha256:02bb6f428431fbc2809c5d1b41eab5a68350194fb508869a33cb1af4444c9b11
```

```
# Docker Compose commands
# Docker Compose is a tool that define and run multiple containers together using a YAML file called docker-compose.yml (or compose.yaml)
# build or rebuild images
docker compose build        # only builds container, not start them
docker compose up -d        # start container
docker compose up --build   # builds container and start them

# Stops the containers but keeps them
docker compose stop

# Stops and removes containers created by Compose
docker compose down

# To remove volumes too
docker compose down -v

# compose logs, -f follow logs live
docker compose logs
docker compose logs -f  

# Restart services
docker compose restart
```

### ✅✅ Container Networking
```
# creating container bridge network
docker network create --driver bridge my-bridge-network

# inspect network
docker network inspect my-bridge-network
docker network ls

# connecting container to bridge network
docker run -d --name container1 --network=my-bridge-network nginx
docker run -d --name container2 --network=my-bridge-network nginx

# creating detailed bridge network
docker network create \
  --driver bridge \
  --label project=dockerinaction \
  --label br-net \
  --attachable \
  --scope local \
  --subnet 10.0.42.0/24 \
  --ip-range 10.0.42.128/25 \
  user-network

# inside docker to examine its network interfaces
ip -f inet -4 -o addr

# connecting a container already connected to a network to another network
docker network connect \
  user-network2 \
  network-explorer

# install and use nmap (Network Mapper) inside network-explorer container
docker exec -it network-explorer sh -c "apk update && apk add nmap"

# scan subnets of both networks inside container
nmap -sn 10.0.42.* 10.0.43.* -oG /dev/stdout | grep Status



```