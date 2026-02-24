# FormadoresIT - Axpe: Kubernetes, OpenShift, ArgoCD, Helm, Jenkins, Tekton

- [FormadoresIT - Axpe: Kubernetes, OpenShift, ArgoCD, Helm, Jenkins, Tekton](#formadoresit---axpe-kubernetes-openshift-argocd-helm-jenkins-tekton)
- [Docker](#docker)
  - [Installation](#installation)
  - [Useful links](#useful-links)
  - [Docker CLI commands](#docker-cli-commands)
    - [Containers](#containers)
      - [docker run](#docker-run)
      - [docker ps](#docker-ps)
      - [docker stop, start, rm](#docker-stop-start-rm)
      - [docker inspect](#docker-inspect)
      - [docker logs](#docker-logs)
      - [docker exec](#docker-exec)
    - [Docker Images](#docker-images)
      - [docker images](#docker-images-1)
      - [docker pull](#docker-pull)
      - [docker login](#docker-login)
      - [docker push](#docker-push)
      - [docker tag](#docker-tag)
      - [docker build](#docker-build)
    - [Dockerfile](#dockerfile)
      - [Multi-stage dockerfiles](#multi-stage-dockerfiles)


# Docker

## Installation

https://docs.docker.com/engine/install/

We used the docker convenience script for installing:

```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh
sudo usermod -aG docker $USER
newgrp docker
docker run hello-world
```

## Useful links

Docker CLI Cheatsheet: https://docs.docker.com/get-started/docker_cheatsheet.pdf

## Docker CLI commands

### Containers

#### docker run

`docker run` creates a new container with the image and parameters set in the command:

```
docker run -d --name nginx nginx:1.29
```

Map ports: `docker run -d --name nginx -p <HOST PORT>:<CONTAINER PORT> nginx:1.29` 

Mount volumes: `docker run -d --name nginx -p 80:80 -v <SOURCE IN HOST>:<DESTINATION IN CONTAINER>:<MODE> nginx`

Remember to always mount read-only files and folders with the `ro` mode.

Set environment variables: `docker run -d -e NGINX_ENTRYPOINT_QUIET_LOGS=1 nginx` 

https://docs.docker.com/reference/cli/docker/container/run/

#### docker ps

`docker ps` lists running containers. `docker ps -a` lists running and stopped containers

https://docs.docker.com/reference/cli/docker/container/ls/

#### docker stop, start, rm

* `docker stop <CONTAINER NAME/ID>` to stop a container. Not deleted, just stopped running. https://docs.docker.com/reference/cli/docker/container/stop/
* `docker start <CONTAINER NAME/ID>` to start a topped container. https://docs.docker.com/reference/cli/docker/container/start/
* `docker rm <CONTAINER NAME/ID>` to delete a stopped container. https://docs.docker.com/reference/cli/docker/container/rm/
* `docker rm -f <CONTAINER NAME/ID>` to delete a running container

#### docker inspect

`docker inspect <CONTAINER NAME/ID>` Prints a JSON with all the information about a container

https://docs.docker.com/reference/cli/docker/container/inspect/


#### docker logs

Prints log output from a container

`docker logs (-f) <CONTAINER NAME/ID>`

https://docs.docker.com/reference/cli/docker/container/logs/

#### docker exec

Runs a command inside a container

`docker exec <CONTAINER NAME/ID> <COMMAND>`  Executes and one time command in the container

`docker exec -ti <CONTAINER NAME/ID> /bin/sh`   Opens a interactive terminal using `sh` in the container

https://docs.docker.com/reference/cli/docker/container/exec/

### Docker Images

#### docker images

`docker images` Lists all images present in the host

https://docs.docker.com/reference/cli/docker/image/ls/

#### docker pull

`docker pull <IMAGE>` pulls (downloads) an image from the registry to the host

https://docs.docker.com/reference/cli/docker/image/pull/

#### docker login

`docker login` Authenticates with a docker registry like DockerHub

https://docs.docker.com/reference/cli/docker/login/

#### docker push

`docker push <IMAGE>` Pushes (uploads) the image to the registry (like DokerHub)

Remember that in DOckerHub you only can push images that starts with your username: <USERNAME>/<IMAGE NAME>

https://docs.docker.com/reference/cli/docker/image/push/

#### docker tag

`docker tag <IMAGE> <NEW TAG>` Adds a new tag (alias) to a given image

https://docs.docker.com/reference/cli/docker/image/tag/


#### docker build

Builds an image from a Dockerfile

```
docker build -t <TAG> (-f <dockerfile_name>) <PATH>
```

Usually, if the file is named Dockerfile and it's in the same directory as the command:

```
docker build -t <TAG> .
```

### Dockerfile

A Dockerfile contains all the steps to build a docker image. Each step is defined by one command (`FROM`, `RUN`, `COPY`, etc). Each new line must start with one of those commands, and it defines a setp in the buidl process.

Here are the most common ones:

`FROM <base image tag>` It's always the first command and defines the base image from which start building the new image

`RUN <COMMAND>` Executes a command during the process

`COPY <SOURCE IN THE HOST> <DESTINATION IN THE IMAGE>`  Copies files or folders from the host to the image

`LABEL <LABEL>` Adds a label to the image. Here are the well-known ones: https://docs.docker.com/engine/manage-resources/labels/

`EXPOSE <PORT>` Defines a port that will be listened by the app in the image

`WORKDIR <PATH>` Sets the root path for all steps below it, including the containers that will run the image

`ENV <VAR>=<VALUE>` Sets environment variables

`USER <ID or NAME>`  Sets the user for all steps below it, including the containers that will run the image

`ENTRYPOINT ["executable", "argument 1", "argument 2" ]`  Defines the command that will run when the container starts

`CMD ["executable", "argument 1", "argument 2" ]`  Defines the command that will run when the container starts. If both ENTRYPOINT and CMD are present, they concatenate to form the command

Full reference: https://docs.docker.com/reference/dockerfile/

ENTRYPOINT vs CMD: https://www.docker.com/blog/docker-best-practices-choosing-between-run-cmd-and-entrypoint/

#### Multi-stage dockerfiles

We can define two or more different parts in a Dockerfile. Each one with its own `FROM` statement. Each stage can contain a different set of tools and run specific commands needed to build the final image. Only the last stage is the one that will survive the build process, it must be as light as possible.

See the example in the java-api folder.

Multi-stage builds: https://docs.docker.com/build/building/multi-stage/


