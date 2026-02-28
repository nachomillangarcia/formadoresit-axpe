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
- [Kubernetes](#kubernetes)
    - [microK8S](#microk8s)
    - [kubectl](#kubectl)
    - [General commands](#general-commands)
    - [Pods](#pods)
    - [Deployments (deploy)](#deployments-deploy)
    - [DeamonSets (ds)](#deamonsets-ds)
    - [Jobs](#jobs)
    - [Cronjobs](#cronjobs)
    - [Services (svc)](#services-svc)
    - [Configmaps (cm)](#configmaps-cm)
    - [Secrets](#secrets)
    - [StatefulSets (sts)](#statefulsets-sts)
    - [Persistence](#persistence)
      - [Persistent Volumes (pv)](#persistent-volumes-pv)
      - [Storage Class (sc)](#storage-class-sc)
      - [Persistent Volume Claim (pvc)](#persistent-volume-claim-pvc)


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



# Kubernetes

### microK8S

Installation: https://microk8s.io/docs/install-alternatives

```
sudo snap install microk8s --classic
sudo usermod -a -G microk8s ubuntu
newgrp microk8s
microk8s status --wait-ready
```

### kubectl

Installation: https://kubernetes.io/docs/tasks/tools/#kubectl

Kubeconfig: https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/

Generate and add kubeconfig for microk8s: `microk8s config > ~/.kube/config`

Create an alias for `k` to be `kubectl`: `echo "alias k=\'kubectl\'" >> .bash_aliases && source .bash_aliases`

**kubectl quick reference**: https://kubernetes.io/docs/reference/kubectl/quick-reference/


### General commands

Create / update objects using YAML file or folder: `kubectl apply -f <FILE>`

List objects `kubectl get <OBJECT>`
    
Options:

- List more details `-o wide`
- Print the YAML `-o yaml`
- Show labels `--show-labels`

Describe a specific object `kubectl describe <OBJECT> <OBJECT_NAME>`

### Pods

Pods are the basic workloads in Kubernetes. They contain one or more containers that share the same network space, meaning they share the same private IP and can communicate between containers using localhost.

https://kubernetes.io/docs/concepts/workloads/pods/

List pods: `kubectl get pod`

List with IP: `kubectl get pod -o wide`

Describe details and events: `kubectl describe pod <POD>`

Get the whole YAML file: `kubectl get pod -o yaml`

Print logs: `kubectl logs <POD> -c <CONTAINER>`

Open terminal into a container: `kubectl exec -ti <POD> -c <CONTAINER> -- bash`

Run a specific command inside container: `kubectl exec <POD> -c <CONTAINER> -- <COMMAND>`

**Add features to containers**:

- Configure probes: https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/

- Set resource requests and limits: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/

- Set environment variables: https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/

- Set command and args (entrypoint and command in Docker): https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/

### Deployments (deploy)

Deployments creates and manages pods that are 100% similar. You can configure how it manages changes.

https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

**Rollout commands**:

- See status and wait for completion: `kubectl rollout status deployment <DEPLOYMENT>`

- Restart all pods: `kubectl rollout restart deployment <DEPLOYMENT>`

- Print past versions: `kubectl rollout history deployment <DEPLOYMENT>`

- Rollback to a version: `kubectl rollout undo deployment <DEPLOYMENT> --to-revision <VERSION>`

ReplicaSets: https://kubernetes.io/es/docs/concepts/workloads/controllers/replicaset/

### DeamonSets (ds)

Deploys 1 pod on all nodes in the cluster

https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

### Jobs

Run a pod that runs a task meant to have an end. Once the tasks inide the containers ends with success, it's marked as completed and does not run anymore. If the task fails, it retries with a configurable policy.

https://kubernetes.io/docs/concepts/workloads/controllers/job/

### Cronjobs

A cronjob creates jobs according to a specific schedule.

https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/


### Services (svc)

Services provides a private IP that balances traffic among similar pods.

There can also open a 

https://kubernetes.io/docs/concepts/services-networking/service/

List endpoints for a service: `kubectl get endpoint <SERVICE> -o yaml>`

DNS address for internal communication with a service: `<SERVICE>.<NAMESPACE>.svc.cluster.local`

### Configmaps (cm)

https://kubernetes.io/docs/concepts/configuration/configmap/

You can use a configmap to mount files into pods and set environment variables.

### Secrets

https://kubernetes.io/docs/concepts/configuration/secret/

Similar to configmap but it obfuscates the content. Meant to be acccessible only to admins and pods.

Special uses for secrets:

- Docker authentication: https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/
  
- TLS certificates for ingresses: https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/


### StatefulSets (sts)

Are similar to deployments. They creates and manages pods but in this case the pods are unique. Statefulsets create pods with an unique identifier. They have an associated service that can route traffic into each pod, not only balance between them. 

Statefulsets can also contain a template to create a PVC for each pod.

Fianally, Statefulsets deploy pods one after one always.

https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/

https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/



### Persistence

#### Persistent Volumes (pv)

It represents a disk or storage server to be used by Kubernetes. Usually they're created atuomatically by a storage provisioner.

https://kubernetes.io/docs/concepts/storage/persistent-volumes/

#### Storage Class (sc)

It represents a storage provisioner that can create persistent volumes and provision the corresponding disk. There are provisioners for all cloud platforms and the most common storage systems.

https://kubernetes.io/docs/concepts/storage/storage-classes/

Easiest example is the storage class for a NFS server: https://kubernetes.io/docs/concepts/storage/storage-classes/#nfs

Microk8s has its own provisioner for OpenEBS that's enabled with this script:

```
sudo apt update && sudo apt install open-iscsi
sudo systemctl enable --now iscsid

microk8s enable community
microk8s enable openebs
```

#### Persistent Volume Claim (pvc)

PVCs are a request for a persistent disk. PVCs are the resource to be used by a pod to mount a persistent volume. They specify the storage class that should provision the persistent volume for the container that mounts it. PVCs survive when the associated pod or pods are deleted.

https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims

Mount a PVC to a pod: https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/