---
title: "What is Docker?"
---

- From [[docker]]

> Docker is a software which provides technology to **isolate** of data or program including some part of OS

<img src="../assets/dockercontainer.png" width="500vw" height="300vw">

## Container and Docker Engine
### Container
- A container is similar to a virtual machine in that it provides an isolated environment for running an appllication
- Unlike a virtual machine, a container does not require a separate operating system to be installed and runs directly on the host system's kernel
- Docker containers are created from Docker images, which are essentially snapshots of a container's file system and configuration at a specific point in time
- Docker images can be build using a Dockerfile
    = Dockerfile: a script that defines the configuration and dependencies of an application
- Docker containers can be managed and orchestrated using Docker Compose, Kubernetes or tother container management tools

### Docker Engine
- Docker Engine is the core component of the Docker platform, responsible for building, running and managing Docker containers.

<hr>

## Why we need to isolate data or program to independent environment?
> By seperating data or programs, you can avoid dependencies on other components and ensure that changes or updates to one component do not affect others
