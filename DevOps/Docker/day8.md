# Docker Compose


Docker Compose is a tool for defining and running multi-container applications. 
It is the key to unlocking a streamlined and efficient development and deployment experience. 

Compose simplifies the control of your entire application stack, making it easy to manage services, networks, and volumes in a single YAML configuration file. Then, with a single command, you create and start all the services
from your configuration file.

Compose works in all environments - production, staging, development, testing, as
well as CI workflows. It also has commands for managing the whole lifecycle of your application:

 - Start, stop, and rebuild services
 - View the status of running services
 - Stream the log output of running services
 - Run a one-off command on a service


## install docker-compose on server 
- https://docs.docker.com/compose/intro/features-uses/

## Install docker 
```
sudo apt update -y
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ubuntu
newgrp docker
sudo chmod 777 /var/run/docker.sock
```
```
docker compose version
```
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```
```
# apt install docker-compose -y
```
- give permission to docker if neede 
```
sudo chmod 777 /var/run/docker.sock
```
- create compose.yaml file in code repository
- `vim compose.yaml` -- create an file 
```yaml
services:
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "8080:8080"

  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "80:80"
    depends_on:
      - backend
```

## docker compose file 
- Create and start containers in detach way 
```
docker compose up -d
```

- Stop and remove resources
```
docker-compose down 
```
- https://docs.docker.com/compose/intro/features-uses/


### Key commands 

To start all the services defined in your `compose.yaml` file:

```console
$ docker compose up
```

To stop and remove the running services:

```console
$ docker compose down 
```

If you want to monitor the output of your running containers and debug issues, you can view the logs with: 

```console
$ docker compose logs
```

To list all the services along with their current status:

```console
$ docker compose ps
```
