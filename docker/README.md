# Docker

## What is Docker?

Docker is a **containerization** platform that packages your application and all its dependencies together in the form of a docker container to ensure that your application works seamlessly in **any environment**.

For a visual explanation, think of the shipping containers used for intermodal shipping. You put your package (code and dependencies) in the container, ship it using a boat or train (laptop or cloud) and when it arrives at its destination it works (runs) just like it did when it was shipped?

## What is Docker container?

Containers are the ready applications created from Docker Images or you can say a Docker Container is a running instance of a Docker Image and they hold the entire package needed to run the application. This happens to be the ultimate utility of Docker.

A Docker container consists of

- A Docker image
- An execution environment
- A standard set of instructions

## What is Docker Image?

Docker Image can be compared to a template which is used to create Docker Containers. They are the building blocks of a Docker Container. These Docker Images are created using the build command.

## Dockerfile

Build an image file : `Dockerfile` 

```
FROM node:8.11.1 # Grab the node carbon image and use it ()

CMD ["/bin/bash"] # CMD stands for command and tells us what to run in the container
```

### Commands

- Build : `docker build -t awesomecontainer .`
- Run our container : `docker run -it awesomecontainer` (`-it` allows us to go "inside" an environment)
- Running in the background : `docker run -it -d awesomecontainer`
- List processes : `docker ps`
- Go back to the container : `docker exec -it [ID] bash`
- Stop the container : `docker stop [ID]`
- Exit the container : `exit`

We need to create the Dockerfile and run commands from the root directory.

**Warning :** It's important to check our containers to make sure our versions are right and all work.

## Pictures

Docker vs VM

<img src="docker_vm.png" width="500px"/>

Docker container 

<img src="docker_container.png" width="500px"/>

Development

<img src="development.png" width="500px"/>

Production

<img src="production.png" width="500px"/>

## Resources

- [Docker Hub](https://hub.docker.com/)
- [A Beginner-Friendly Introduction to Containers, VMs and Docker](https://medium.freecodecamp.org/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b)