# Docker

## What is Docker?

Docker provides an environment to containerize an app (Python, JS, ...) by creating a self-sufficient autonomous image that contains the app itself as well as all its dependencies.

It is particularly useful to quickly push code into production and makes it very convenient to setup CD.

## Basic Docker commands

### The tag system

Docker uses a tag system to identify different versions of a same Docker image. It is particularly useful when deploying different versions of the same code on a server.

```
docker build -f Dockerfile -t pabannier/my_wonderful_project:latest .
```

This command builds an image using a file called `Dockerfile` with the name `my_wonderful_project` and the tag `latest`.

```
docker run pabannier/my_wonderful_project:latest
```

This command will run the actual image. If you don't include the tag, by default Docker implies the tag latest.

`docker ps`: shows the running containers.

`docker ps -a`: shows the running and past running containers.

`docker rm <NAME_OF_CONTAINER>`: removes a container with name <NAME_OF_CONTAINER>.

`docker rmi <NAME_OF_IMAGE>`: removes an image with name <NAME_OF_IMAGE>.

`docker logs <CONTAINER_ID>`: displays the logs of the container <CONTAINER_ID>.

### Docker-compose file

Docker-compose file is a YAML file that allows you to simultaneously launch multiple Docker images, as well as configure them so that they run on the same network but on different ports.
