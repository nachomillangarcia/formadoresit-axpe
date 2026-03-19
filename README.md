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
    - [RBAC (Role Based Access Control)](#rbac-role-based-access-control)
      - [Service Accounts (sa)](#service-accounts-sa)
      - [Roles and Clusterroles](#roles-and-clusterroles)
      - [RoleBindings and ClusterRoleBindings](#rolebindings-and-clusterrolebindings)
- [Helm](#helm)
    - [Installation](#installation-1)
    - [Core concepts](#core-concepts)
    - [Commands](#commands)
    - [Helm Charts](#helm-charts)
      - [Template syntax](#template-syntax)
      - [Hooks](#hooks)
    - [Managing Repositories](#managing-repositories)
      - [Classic repositories](#classic-repositories)
      - [OCI repositories](#oci-repositories)
      - [Helm chart tests](#helm-chart-tests)
    - [Helm CLI plugins](#helm-cli-plugins)
- [Log management with ElasticSearch stack](#log-management-with-elasticsearch-stack)
    - [Installing](#installing)
      - [ElasticSearch cluster](#elasticsearch-cluster)
      - [Kibana](#kibana)
    - [Filebeat](#filebeat)
      - [Deploying](#deploying)
      - [Config example](#config-example)
      - [Annotations](#annotations)
      - [References](#references)
    - [Kibana](#kibana-1)
- [Monitoring with Prometheus and Grafana](#monitoring-with-prometheus-and-grafana)
    - [Deploying](#deploying-1)
    - [Queries](#queries)
      - [Selectors](#selectors)
      - [Functions](#functions)
      - [Operators](#operators)
    - [Metric types](#metric-types)
      - [Gauge](#gauge)
      - [Counter](#counter)
      - [Histogram](#histogram)
      - [Summary](#summary)
    - [Configuration](#configuration)
    - [Metrics sources (jobs and ServiceMonitor)](#metrics-sources-jobs-and-servicemonitor)
    - [Rules (PrometheusRule)](#rules-prometheusrule)
      - [Recording rules](#recording-rules)
      - [Alerting rules](#alerting-rules)
    - [Alert Mamager](#alert-mamager)
      - [Alert Manager Discovery](#alert-manager-discovery)
      - [Configuration](#configuration-1)
    - [Exporters](#exporters)
    - [Grafana](#grafana)
      - [Connect with Prometheus](#connect-with-prometheus)
      - [Import dashboards for node-exporter and kube-state-metrics](#import-dashboards-for-node-exporter-and-kube-state-metrics)
      - [Create new dashboards](#create-new-dashboards)
      - [Variables](#variables)
- [External Secrets Operator](#external-secrets-operator)
    - [Installing](#installing-1)
    - [SecretsStore and ClusterSecretsStore](#secretsstore-and-clustersecretsstore)
    - [ExternalSecret and ClusterExternalSecrets](#externalsecret-and-clusterexternalsecrets)
    - [Alternatives](#alternatives)
- [Openshift](#openshift)
  - [Installation and distributions](#installation-and-distributions)
  - [oc CLI](#oc-cli)
  - [Specific Openshift features](#specific-openshift-features)
    - [Projects](#projects)
    - [Routes](#routes)
    - [Internal Image Registry](#internal-image-registry)
    - [ImageStreams](#imagestreams)
    - [BuildConfigs and Builds](#buildconfigs-and-builds)
- [Jenkins in Kubernetes](#jenkins-in-kubernetes)
  - [Installation in Openshift](#installation-in-openshift)
  - [Kubernetes Cloud Plugin](#kubernetes-cloud-plugin)
    - [Configuration](#configuration-2)
    - [Static Pod Templates](#static-pod-templates)
  - [Jenkinsfile](#jenkinsfile)
    - [Agent](#agent)
    - [Stages](#stages)
  - [Jenkins Pipeline Jobs](#jenkins-pipeline-jobs)
- [Tekton Pipelines](#tekton-pipelines)
  - [Installation](#installation-2)
  - [Tekton Pipeline Object reference](#tekton-pipeline-object-reference)
    - [Task](#task)
      - [Tasks catalog](#tasks-catalog)
    - [TaskRun](#taskrun)
    - [Pipeline](#pipeline)
    - [PipelineRun](#pipelinerun)
  - [Tekton Triggers object reference](#tekton-triggers-object-reference)
    - [Trigger Template](#trigger-template)
    - [Trigger Binding](#trigger-binding)
    - [Trigger](#trigger)
    - [Event Listener](#event-listener)
- [ArgoCD](#argocd)
    - [Installing](#installing-2)
    - [Projects](#projects-1)
    - [Applications](#applications)
- [Next steps](#next-steps)
  - [Networking projects](#networking-projects)
    - [Gateway API](#gateway-api)
    - [External DNS](#external-dns)
    - [Cert Manager](#cert-manager)
  - [Dashboards](#dashboards)


**IMPORTANT: Find the example Java API and Jenkinsfile code in the extra repository: https://github.com/nachomillangarcia/axpe-java-api** 

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

### RBAC (Role Based Access Control)

#### Service Accounts (sa)

A service account is just a token a pod (or many) can use to communicate with Kubernetes API

https://kubernetes.io/docs/concepts/security/service-accounts/

To define a new service account:

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-serviceaccount
  namespace: my-namespace
```

To use the service account in a deployment, for example:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        ...
      volumes:
        ...
      serviceAccountName: nginx
```

The token from the service account is mounted on the path `/var/run/kubernetes.io/`  along with the root certificate (`ca.crt`) to communicate with the Kubernetes API. Containers within the Kubernetes cluster can user the address `kubernetes.default.svc.cluster.local` as the endpoint.

#### Roles and Clusterroles

Are a list of permissions. Roles are namespaced while clusterroles are cluster-wide

https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-and-clusterrole

```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: secret-reader
rules:
- apiGroups: [""]          # Same as apiVersion field for the object we want to grant permissions, without the version. "" means v1
  resources: ["secrets"]   # Object names over which grant permissions
  verbs: ["get", "watch", "list"]  # List of verbs
```

#### RoleBindings and ClusterRoleBindings

Grant a single role to one or more users/groups/service accounts. Rolebindings are namespaced while clusterRoleBindings are cluster-wide

https://kubernetes.io/docs/reference/access-authn-authz/rbac/#role-binding-examples

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods
  namespace: default
subjects:
# You can specify more than one "subject"
- kind: ServiceAccount       # this must be User, Group or ServiceAccount
  name: my-serviceaccount 
  apiGroup: rbac.authorization.k8s.io
roleRef: 
  kind: Role                 # this must be Role or ClusterRole
  name: secret-reader        # name of the Role of ClusterRole
  apiGroup: rbac.authorization.k8s.io
```

# Helm

### Installation

```
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
```

https://helm.sh/docs/intro/install/

### Core concepts

- **Chart**: An application to be deployed using Helm
- **Release**: A chart deployed to Kubernetes
- **Values**: Variables used to customize a release
- **Repository**: An online source for charts. Always use official repositories

### Commands

Reference: https://helm.sh/docs/intro/using_helm/


Repositories:

- Add a new repository: `helm repo add bitnami https://charts.bitnami.com/bitnami`
- Update all repositories: `helm repo update`

Install / Upgrade:

- `helm install <RELEASE NAME> <REPOSITORY>/<CHART>`
- Upgrade: `helm upgrade <RELEASE NAME> <REPOSITORY>/<CHART>`
- Use a local chart `helm install/upgrade <RELEASE NAME> <CHART FOLDER>`
- Specify namespace `-n namespace`
- Specify values file `--values <FILE>`

Print redered YAMLs without actually deploying:
- `helm template <RELEASE NAME> <REPOSITORY>/<CHART>`

List releases:

- `helm ls -n <NAMESPACE>`

List revisions for a release:

- `helm history -n <NAMESPACE> <RELEASE NAME>`

Print values:

- `helm get values -n <NAMESPACE> <RELEASE NAME>`
- Print all values including defaults `--all`
- Specific revision `--revision <REVISION NUMBER>`

Print Kubernetes YAMLs:

- `helm get manifest -n <NAMESPACE> <RELEASE NAME>`
- Print all values including defaults `--all`
- Specific revision `--revision <REVISION NUMBER>`



### Helm Charts

Chart structure and common files: https://helm.sh/docs/topics/charts/

Kickstart a new chart: https://helm.sh/docs/chart_template_guide/getting_started/

Dependencies and sub-charts: https://helm.sh/docs/chart_best_practices/dependencies/

#### Template syntax

Generealities: https://helm.sh/docs/chart_best_practices/templates/

Using built-in variables as `.Release` and `.Chart`: https://helm.sh/docs/chart_template_guide/builtin_objects/

Using Values `{{ .Values.... }}`: https://helm.sh/docs/chart_template_guide/values_files/

Conditionals (`if`): https://helm.sh/docs/chart_template_guide/control_structures/#ifelse

```
# Condition for Value being true

{{- if .Values.... }}

{{- end }}

# Compare a Value. In this example, to be equal to other part.
# You can use the operators [eq, ne, lt, gt, and, or] into conditional statements

{{- if eq .Values... <EQUAL VALUE>}}

{{- end }}
```

Looping (`range`): https://helm.sh/docs/chart_template_guide/control_structures/#looping-with-the-range-action

```
# Over a list:

{{- range .Values.deployment.volumeMounts }}
  - name: {{ .name | quote }}
    mountPath: {{ .path | quote }}
{{- end }}

# Over a dictionary

{{- range $name, $value := .Values.deployment.annotations }}
{{ $name }}: {{ $value }}
{{- end }}
```

Modifying the scope (`with`): https://helm.sh/docs/chart_template_guide/control_structures/#modifying-scope-using-with

```
# Set the new scope to .Values.Service
  {{- with .Values.service}}
  type: {{ .type | default "ClusterIP" }}
    {{- if eq .type "NodePort" }}
    nodePort: {{ .nodePort }}
    {{- end }}
    targetPort: http
  {{- end }}
{{- end }}
```

Note that you can always access to the root scope using the reserved variable `$`: `$.Release.Name`

Define variables (`{{- $relname := .Release.Name -}}`): https://helm.sh/docs/chart_template_guide/variables/

Functions: https://helm.sh/docs/chart_template_guide/functions_and_pipelines/

```
# Use a functions with parameters

{{ quote .Values.favorite.drink }}  # This adds quotes in the extremes of the value

# Use a function like pipeline

{{ .Values.favorite.food | upper | quote }} # This makes the value uppercase, and then adds quotes
```

Full functions list: https://helm.sh/docs/chart_template_guide/function_list/

#### Hooks

Hooks allow to deploy some objects before or after all the others.

We define hooks adding annotations to the objects we want Helm to deploy before or after:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Release.Name }}
  namespace: {{ .Release.Namespace }}
  annotations:
    "helm.sh/hook": pre-install,pre-upgrade   # Deploy this configmap before other resources when installing or upgrading
    "helm.sh/hook-weight": "-5"               # Set a weight to control the order among all the other objects with the same hook
```

Default deploy order in Helm:  https://helm.sh/docs/intro/using_helm/#helm-install-installing-a-package

All different hooks and reference: https://helm.sh/docs/topics/charts_hooks/#helm

### Managing Repositories

#### Classic repositories

https://helm.sh/docs/topics/chart_repository/

Helm repositories are merely web servers with the chart files and an index.yaml file that Helm understands to retrieve the charts.

To create a helm repository:

```
helm repo package <PATH TO CHART>     # Create a .tgz file with the chart 
mv <TGZ FILE> ./<REPOSITORY FOLDER>   # Move it to the folder for the repo
helm repo index ./<REPOSITORY FOLDER> # Generate or update index file
<upload the repository folder to a file server>
```

#### OCI repositories

https://helm.sh/docs/topics/registries/

New OCI repositories doesn't require all the steps before. We can just push our chart to a OCI-compliant registry (DockerHub, GitHub, etc.)

First we need to log in to the registry using `helm login`

The we can just push the chart to the registry:

``` helm push <.tgz file> <oci:// URL>```

#### Helm chart tests

https://helm.sh/docs/topics/chart_tests/

We can set up pods that runs tests for our helm charts. For example, to check if connection to service is up. To do that, we create a new YAML in the /templates folder with a pod resource that run the tests, and add the annotation `"helm.sh/hook": test`.

To run the test pods in a realese, just run the command `helm test <REALEASE>`. Tests are meant to run during development.

### Helm CLI plugins

There are plugins that add funcionalities to the `helm` CLI. To install a plugin, use the command `helm plugins install <GITHUB REPO FOR THE PLUGIN>`.

Here is a full list of useful helm plugins: https://helm.sh/docs/community/related/#helm-plugins

In the course we installed the plugin helm diff: https://github.com/databus23/helm-diff

# Log management with ElasticSearch stack

### Installing

#### ElasticSearch cluster

First install the ECK operator (https://www.elastic.co/docs/deploy-manage/deploy/cloud-on-k8s) :

```
helm repo add elastic https://helm.elastic.co
helm install elastic-operator elastic/eck-operator -n elastic-system --create-namespace
```

Then we create the elastic namespace and a secret with the user and password:

```
kubectl create ns elastic
kubectl -n elastic create secret generic elasticsearch-es-elastic-user  --from-literal=elastic=elastic
```

And deploy the YAML with the file `Elasticsearch` object:

```
kubectl apply -f elasticsearch/elasticsearch.yaml
```


#### Kibana

```
kubectl apply -f elasticsearch/kibana.yaml
```

### Filebeat

Filebeat is an agent that reads log files from plenty of different sources, process them line by line, and push the processed objects to ElsticSearch for storage and querying.

Filebeat comes with pod autodiscovery to be used in Kubernetes easily:

https://www.elastic.co/docs/reference/beats/filebeat/configuration-autodiscover

#### Deploying

```
kubectl apply -f elasticsearch/filebeat.yaml
```

This deploys filebeat in a daemonset with the default configuration for Kubernetes.

#### Config example

```      
  config:
    http:                   # Enable HTTP port for metrics
      enabled: true
      host: 0.0.0.0
      port: 5066
    filebeat:
      autodiscover:                 # Configures Kubernetes hints-based autodiscovery
        providers:
        - type: kubernetes
          node: ${NODE_NAME}
          hints:
            enabled: true
            default_config:         # Default config if no other templates matches the pod
              type: filestream
              id: container-${data.container.id}
              paths:
              - /var/log/containers/*${data.kubernetes.container.id}.log       # Path to the logs folder for the node
              parsers:
              - container: {}               # Parse the content of the logs files with the container module
              close.inactive: 5m
              close.removed: true
              close.on_state_change.removed: false
              ignore_older: 30m
              clean_inactive: 48h
              prospector:
                scanner:
                  fingerprint.enabled: false
                  symlinks: true
                  check_interval: 10s        # how often to rescan
                  exclude_files: ['\.gz$']   # skip rotated files
```

#### Annotations

Hints-based autodiscovery allows a set of annotations to be added to the pods so we can modify the behaviour of filebeat for those pods: https://www.elastic.co/docs/reference/beats/filebeat/configuration-autodiscover-hints

To monitor Nginx using filebeat Nginx module:

```
        co.elastic.logs/module: "nginx"
        co.elastic.logs/fileset.stdout: "access"
```

To parse logs from CoreDNS pod using the disect processor:

```
    co.elastic.logs/processors.dissect.tokenizer: '[%{log.level}] %{client_ip}:%{client_port} - %{connection_id} "%{query.class} IN %{query.name} %{query.type} %{qid} false %{length}" %{rcode} %{flags} %{number} %{response_time}s'
    co.elastic.logs/processors.dissect.field: "message"
    co.elastic.logs/processors.dissect.ignore_failure: "true"
    co.elastic.logs/processors.dissect.target_prefix: "coredns"
```

#### References

- Filebeat: https://www.elastic.co/docs/reference/beats/filebeat/
- Inputs: https://www.elastic.co/docs/reference/beats/filebeat/configuration-filebeat-options
  - Container input: https://www.elastic.co/docs/reference/beats/filebeat/filebeat-input-container
- Modules: https://www.elastic.co/docs/reference/beats/filebeat/filebeat-modules
  - Nginx module: https://www.elastic.co/docs/reference/beats/filebeat/filebeat-module-nginx
- Processors: https://www.elastic.co/docs/reference/beats/filebeat/filtering-enhancing-data
  - Dissect processor: https://www.elastic.co/docs/reference/beats/filebeat/dissect


### Kibana

Kibana is the web dashboard to use with ElasticSearch.

Once we've deployed filebeat to push the first documents to ElasticSearch, we can enter Kibana, go to `discover`  and create a new **data view** to start querying the documents: https://www.elastic.co/docs/explore-analyze/find-and-organize/data-views

Then we can start using the **Discover** page to explore the data: https://www.elastic.co/docs/explore-analyze/discover/discover-get-started

A very useful feature briefly explored in the course are Dashboards, in which you can create visualizations from queries: https://www.elastic.co/docs/explore-analyze/dashboards

# Monitoring with Prometheus and Grafana

Prometheus is the default metrics server in Kubernetes. All components in a Kubernetes cluster expose metrics in the format defined for Prometheus. 

Grafana is the web dashboard that integrates easily with Prometheu and many other data sources.

### Deploying

Using Prometheus Operator (https://prometheus-operator.dev/) and the helm chart https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack

```
kubectl create namespace monitoring
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring --values helm/prometheus-values.yaml
```

This will install:
- Prometheus operator and the necessary CustomResourceDefinitions
- It creates an object `kind: Prometheus` with the passed specification. The operator then deploys the Prometheus cluster according to the spec.
- Same for `kind: Alertmanager` for the alertmanager
- A daemonset for nodeexporter (https://github.com/prometheus/node_exporter) which exposes metrics about nodes health
- A deployment for kube-state-metrics which is a generic agent for Kubernetes that exposes metrics about different objects and their status (deployments, jobs, statefulsets, pods, etc.)
- A deployment for Grafana
- A bunch of default monitors to tell Prometheus to monitor all those components by default.

### Queries

#### Selectors

https://prometheus.io/docs/prometheus/latest/querying/basics/#time-series-selectors

Prometheus metrics can be selected using its labels

```
container_memory_working_set_bytes{image!="registry.k8s.io/pause:3.10", container!="", namespace="default",pod=~"nginx-deployment-.*"}
```

This query excludes images equal to "registry.k8s.io/pause:3.10", excludes results where container label is empty, selects only the results with namespace label = default and with pod label similar to nginx-deployment

#### Functions

https://prometheus.io/docs/prometheus/latest/querying/functions/

We can apply functions to metrics to make calculations, averages, etc

```
rate(container_cpu_usage_seconds_total{image!="registry.k8s.io/pause:3.10", container!="", namespace="default",pod=~"nginx-deployment-.*"} [5m])
```

Calculates the rate of change for the metric container_cpu_usage_seconds_total

#### Operators

https://prometheus.io/docs/prometheus/latest/querying/operators

There are arithmetic operators (+, -, *, /, etc) and comparison operators (>, <, ==, etc)

```
container_memory_working_set_bytes{image!="registry.k8s.io/pause:3.10", container!="", namespace="default",pod=~"nginx-deployment-.*"} / (1024*1024)
```

Divides the metric by (1024*1024) to convert it to Mebibytes

We can add modifiers to operators to help matching labels between metrics:

https://prometheus.io/docs/prometheus/latest/querying/operators/#vector-matching

```
container_memory_working_set_bytes{image!="registry.k8s.io/pause:3.10", container!="", namespace="default",pod=~"nginx-deployment-.*"} / on(pod, namespace, container) kube_pod_container_resource_limits{resource="memory"}
```

Divides memory usage for containers by the memory limit for the container, and does the matching using only the labels (pod, namespace, container)


### Metric types

#### Gauge

https://prometheus.io/docs/concepts/metric_types/#gauge

Represents the exact value at the time when the metric was scraped. i.e. memory usage at that time, disk usage at that time, etc

It can go up and down.

We don't need to do any other operations to make use of the value. It already represents the information we're seeking

#### Counter

https://prometheus.io/docs/concepts/metric_types/#counter

Counter only goes up. It's meant to represent a metric that's always increasing, as CPU usage or requests. Metrics are created as counters to give information not only about the moment like the gauges for memory usage, but to give information about how it has increased over time.

We use rate functions like rate, irate, delta, idelta to extract information about how the counter metric has changed, so we get requests per second or core usage per second, instead of total requests or total cpu usage.

```
rate(container_cpu_usage_seconds_total{image!="registry.k8s.io/pause:3.10", container!="", namespace="default",pod=~"java-.*"} [5m])
```

`container_cpu_usage_seconds_total` is the total cpu seconds since the pod started. rate() transforms it in the average of seconds used in the last 5 min


#### Histogram

https://prometheus.io/docs/concepts/metric_types/#histogram

https://prometheus.io/docs/practices/histograms/

Are counters that are disgregated in different buckets, under the label `le` 

```
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.00025"} 257
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.0005"} 464
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.001"} 495
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.002"} 495
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.004"} 548
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.008"} 561
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.016"} 610
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.032"} 613
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.064"} 614
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.128"} 614
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.256"} 614
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="0.512"} 723
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="1.024"} 729
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="2.048"} 729
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="4.096"} 730
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="8.192"} 730
coredns_dns_request_duration_seconds_bucket{server="dns://:53",view="",zone=".",le="+Inf"} 730
coredns_dns_request_duration_seconds_sum{server="dns://:53",view="",zone="."} 61.89406280700003
coredns_dns_request_duration_seconds_count{server="dns://:53",view="",zone="."} 730
```

In `coredns_dns_request_duration_seconds_bucket` each one of these metrics represents how many queries has been served below `<le>` seconds.

Histograms always contain +Inf le meaning the total of requests served. They include also a sum and count.

We use this metrics along with the function histogram_quantile to get percentiles.

```
histogram_quantile(0.95, sum(idelta(coredns_dns_request_duration_seconds_bucket[5m])) by (le))
```

#### Summary

https://prometheus.io/docs/concepts/metric_types/#summary

Similar to histogram, but it already provides the percentiles so we don't need to apply the histogram_quantile function

### Configuration

Prometheus config file contains the parameters to tell the server. We can provide a config file, but the Prometheus Operator defines custom object that will add configs to the server automatically.

https://prometheus.io/docs/prometheus/latest/configuration/configuration/

https://prometheus-operator.dev/docs/api-reference/api/


### Metrics sources (jobs and ServiceMonitor)

Prometheus scrape metrics from the configured targets every 1 minute (by default). A job defines a target to be scraped.

In a setup with Proemtheus Operator as ours, we configure jobs creating a resource of kind ServiceMonitor: https://prometheus-operator.dev/docs/api-reference/api/#monitoring.coreos.com/v1.ServiceMonitor

```
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  endpoints:
  - port: metrics

  selector:
    matchLabels:
      app: nginx
  namespaceSelector:
    matchNames:
    - default
```

A ServiceMonitor configures Prometheus to scrape metrics from endpoints (pods in Ready state that receives traffic from a service) that match the selector values.

We can see the configured targets, with their URLs and health status in the we UI, in the targets section.

The label `job` tells what serviceMonitor or job has provided the metric.

### Rules (PrometheusRule)

Rules are pre-defined queries that run periodically in the server. We can store the result in a new metric to be used (Recording Rules) or use the rule to trigger an alert (Alerting Rules).

#### Recording rules

https://prometheus.io/docs/prometheus/latest/configuration/recording_rules/

```
    groups:
      - name: node-requests.rules
        rules:
        - record: node:memory_requests:ratio
          expr: |-
            (sum by (node) (kube_pod_container_resource_requests{resource="memory"})/ 1024 / 1024) 
            /
            (sum by (node) (kube_node_status_allocatable{resource="memory"})/ 1024 / 1024) * 100
```

Recording rules contain a `expr` with the query to be run periodically and a `record` to store the result. We can then use the record as a metric anywhere.

#### Alerting rules

https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/

```      
        rules:
        - alert: NodeMemoryRequestsHigh
          annotations:
            description: |
              {{ $labels.node }} node memory requests are over 3%, currently at {{$value }}%
            summary: Node has high memory requests
          expr: node:memory_requests:ratio > 3
          for: 5m
          labels:
            severity: warning
            node: |-
              {{ $labels.node }}
```

Alerting rules contain a `expr` field with the query to be run. Also

- `for` is how long it'll wait to trigger the alert
- labels and annotations to add info to the alert triggered
  
This alert will triger if the `expr` returns any value for `5m`

If we have a Prometheus operator, we add rules by creating a file of kind `prometheusRule`: https://prometheus-operator.dev/docs/api-reference/api/#monitoring.coreos.com/v1.PrometheusRule


### Alert Mamager

AlertManager is the component of Prometheus that receives triggering alerts from one or many Prometheus servers and notify configured receivers.

#### Alert Manager Discovery

Promtheus can auto-discover alert managers to interact with, by using this section in Prometheus configuration: https://prometheus.io/docs/prometheus/latest/configuration/configuration/#alertmanager_config

#### Configuration

https://prometheus.io/docs/alerting/latest/configuration

All features in alert manager are set in the configuration file. Sections

- global: global settings and endpoints for different receivers
- routes: list of rules to match triggering alerts with the coresponding receiver
- receivers: list of receivers to be used in routes
- inhibitors: rules to suspend alerts if there are other alerts that meet the conditions (i.e. if there are critical alerts, don't send warnings)

### Exporters

As not every software is made to expose metrics for Prometheus, there are exporters that make this work for us. Here is a full list of available exporters: https://prometheus.io/docs/instrumenting/exporters/

We deployed Nginx exporter (https://hub.docker.com/r/nginx/nginx-prometheus-exporter) in the `deploment-nginx.yaml` file and added the corresponding service monitor.

### Grafana

#### Connect with Prometheus

1st create a data source for Prometheus, connect with the deployed cluster using `http://prometheus-kube-prometheus-prometheus.svc.cluster.local:9090`

#### Import dashboards for node-exporter and kube-state-metrics

We can jump into dashboards by importing existing ones available in https://grafana.com/grafana/dashboards/

One example is https://grafana.com/grafana/dashboards/1860-node-exporter-full/, we can import it in New > Import and use the ID for he dashboard.

#### Create new dashboards

https://grafana.com/docs/grafana/latest/dashboards/build-dashboards/

We can add new visualizations for our queries. Each visualization can contain more than one query. It's important to:

* Configure the units of the visualization
* Configure the legend for each query so it's readeable. We can us labels with `{{ label }}` in the legend.

#### Variables

A central feature of dashboards is setting up variables to select what resources are plotted in the dashboard.

https://grafana.com/docs/grafana/latest/dashboards/variables/

Variables can get the list of options dinamically using prometheus queries. Usually we will want to use the Query type: Label values, and the metric `kube_pod_info` to get a list of pods and namespaces in the cluster. We can use similar queries for nodes, deployments, etc. See the example dashboard.

Variables can be ordered and used in the queries that set up other variables, so we get for example only the pods that belongs to the selected namespace.

Then, we can use the variables in the queries for the visualizations: `$variable_name` 



# External Secrets Operator

External Secrets Operator synchronises secrets stored in an external source (AWS secrets manager, etc) to secrets in the Kubernetes cluster.

### Installing

```
helm repo add external-secrets https://charts.external-secrets.io
helm install external-secrets external-secrets/external-secrets -n external-secrets --create-namespace
```

### SecretsStore and ClusterSecretsStore

https://external-secrets.io/latest/api/secretstore/

Defines the integration with the external secrets source:

```
apiVersion: v1
kind: Secret
metadata:
  name: awssm-secret
data:
  access-key: <AWS ACCESS KEY>
  secret-access-key: <AWS SECRET KEY>
---

apiVersion: external-secrets.io/v1
kind: SecretStore
metadata:
  name: aws-secretsmanager
spec:
  provider:
    aws:
      service: SecretsManager
      region: eu-central-1
      auth:
        secretRef:
          accessKeyIDSecretRef:
            name: awssm-secret
            key: access-key
          secretAccessKeySecretRef:
            name: awssm-secret
            key: secret-access-key
```

In the example we store the relevant credentials in a Secret, and then create the SecretStore for AWS secrets manager, referencing the secret for credentials.

### ExternalSecret and ClusterExternalSecrets

https://external-secrets.io/latest/api/externalsecret/

Describes what data should be fetched, how the data should be transformed and saved as a Secret in Kubernetes-

```
apiVersion: external-secrets.io/v1
kind: ExternalSecret
metadata:
  name: example
spec:
  refreshInterval: 12h
  secretStoreRef:
    name: aws-secretsmanager
    kind: SecretStore
  target:
    name: secret-to-be-created
    creationPolicy: Owner
  dataFrom:
  - extract:
      key: formadoresit 
```

The example extracts the data stored in the secret named `formadoresit` in AWS secrets manager, and stores the data in the target Kubernetes secret `secret-to-be-created`

We can then mount the secret in the pods.

### Alternatives

Storing credentials in Kubernetes secrets is still not so secure, as those are not encrypted.

Ideally our pods should read directly secrets from the external source, bypassing kubernetes secrets completely.

To automate this process we can use Secrets Store CSI driver: https://secrets-store-csi-driver.sigs.k8s.io/

It's a controller that creates a new kind of volume that we can mount into the pods. At runtime, this kind of volume retrieves the secrets for the external source and mounts them into the pod, with no extra objects.

# Openshift

## Installation and distributions

Openshift is a distribution of Kubernetes, which adds operators for quality of life for admins and developers, and enterprise support (security updates, etc).

Maintained by Red Hat, it's easy to deploy Openshift clusters on-premise and on any cloud: https://docs.redhat.com/en/documentation/openshift_container_platform/4.10/html/installing/ocp-installation-overview 

We can deploy it in local using CDC(Code Ready Containers): https://console.redhat.com/openshift/create/local

Or using the open source upstream version of Openshift: OKD https://docs.okd.io/4.17/architecture/architecture-installation.html#architecture-installation

During the course we used the developer sandbox for Openshift, which is a ready-to-use environment provided by Red Hat: https://developers.redhat.com/developer-sandbox

## oc CLI

oc CLI is the tool for the terminal to manage openshift. We followed these steps to install it on Linux:

```
curl -L https://mirror.openshift.com/pub/openshift-v4/clients/ocp/stable/openshift-client-linux.tar.gz | \
tar -xzf - -C /tmp && sudo mv /tmp/oc /usr/local/bin/oc && sudo chmod +x /usr/local/bin/oc
```

To connect to an OpenShift cluster, we use the `oc login` command with the cluter URL and token. It automatically adds entries in our kubeconfig so `oc` and  `kubectl` can interact with the Openshift cluster.

## Specific Openshift features

### Projects

https://docs.redhat.com/en/documentation/openshift_container_platform/4.8/html/building_applications/projects

In OpenShift, a Project is essentially a Kubernetes Namespace with additional features and metadata layered on top by OpenShift.

It deployes a bunch of objects alongside with the namespace: resource quotas, network policies, roles and role bindings, etc.

### Routes

https://docs.redhat.com/en/documentation/openshift_container_platform/4.11/html/networking/configuring-routes

Routes are the Openshift replacement for Gateway API in Kubernetes. It allows to expose services using a single entrypoint for all traffic into the cluster. It manages domains and TLS termination. We used Routes to expose services in Openshift.

### Internal Image Registry

https://docs.redhat.com/en/documentation/openshift_container_platform/4.10/html/registry/registry-overview-1

Openshift features it's own image registry for Docker, so we don't need to use an external registry. It lives in the internal URL `image-registry.openshift-image-registry.svc:5000`.

### ImageStreams

https://docs.redhat.com/en/documentation/openshift_container_platform/4.10/html/images/managing-image-streams

Represents docker images in its internal resgitry. ImageStreams integrates with deployments and any workload, allowing automatic updates of pods when we push a new version of the image. 

We did that during the training by adding an anotation to the java-api deployment:

```
  annotations:
    image.openshift.io/triggers: |-
      [
        {
          "from": {
            "kind": "ImageStreamTag",
            "name": "java-api:latest"
          },
          "fieldPath": "spec.template.spec.containers[?(@.name==\"axpe-java-api\")].image"
        }
      ]
```

### BuildConfigs and Builds

https://docs.redhat.com/es/documentation/openshift_container_platform/4.2/html/builds/understanding-buildconfigs

A BuildConfig is an OpenShift resource that defines how to build a container image. It's essentially a blueprint/template for builds — it tells OpenShift where to get the source code, how to build it, and where to push the resulting image.

During the training, we created a buildconfig that receives a folder (source: binary), builts the Dockerfile in the API repository and pushes to an image stream:

```
kind: BuildConfig
apiVersion: build.openshift.io/v1
metadata:
  name: java-api
  namespace: nachomillangarcia-dev
spec:
  nodeSelector: null
  output:
    to:
      kind: ImageStreamTag
      name: 'java-api:latest'
  strategy:
    type: Docker
  source:
    type: Binary
```

There are a bunch of other features and objects present in Openshift, related to security, resource management, networking, CI/CD, etc.

# Jenkins in Kubernetes

Jenkins is an extensive tool and we explored just the very basics of it, focusing on its integration with Kubernetes

## Installation in Openshift

Openshift provides templates to deploy a Jenkins version that includes important plugins and configurations to integrate with Kubernetes and Openshift platforms: https://github.com/redhat-cop/openshift-templates/blob/master/jenkins/jenkins-persistent-template.yml

Template variables used:

- `Enable OAuth in Jenkins  = false`
- `Memory = 2Gi`
- `Delete the nodejs and java images`
- `Disable memory intensive administrative monitors = true`

There is an official Helm chart to deploy it in any Kubernetes cluster: https://github.com/jenkinsci/helm-charts

## Kubernetes Cloud Plugin

https://plugins.jenkins.io/kubernetes/

This is a necessary plugin to integrate Kubernetes and Jenkins. The plugin basically allows to deploy agents dinamically, using pod definitions and templates. Thanks to that feature, we can define the agents in Jenkinsfile or in statis podTemplates, that will spawn only when needed and terminate after it's completed, so we avoid wasting resources on unused agents while there are no jobs for them.

### Configuration

Kubernetes plugin configuration page is in `Manage Jenkins > Nodes and Clouds > Clouds`.

Basic configuration includes connection details to our cluster:

* URL: `kubernetes.default.svc.cluster.local`
* CA Certificate
* credentials (service account)

### Static Pod Templates

Pod Template are pod definitions that will spawn to build specific jobs. Within the pod template, we can configure different containers that may be used in those jobs.

The `labels` section defines the labels to be referenced in the Jenkinsfile to build that job in the pod template.

There is one container that must be always present: `agent`. This container runs the Jenkins agent that connects the new pod to the jenkisn server. The plugin can inject the agent for us, or we can create a custom container image that includes the agent.

Apart from that one container, we can add more containers for different tools that may be used in the Jenkinsfile's stages

We can add environment variables, volumes, securityContext, etc to our pod template.

## Jenkinsfile

A Jenkinsfile is a file that lives along with the application code and contains definitions of all the steps needed to build our application (unti tests, package installation, docker builds, etc).

It's important to note that there are two different syntaxes:

* Scripted sntax: https://www.jenkins.io/doc/book/pipeline/syntax/#scripted-pipeline  This one is kinda deprecated.
* Declarative syntax: https://www.jenkins.io/doc/book/pipeline/syntax/#declarative-pipeline  This is the current one, which we used during the training.

### Agent

https://plugins.jenkins.io/kubernetes/#plugin-content-declarative-pipeline

The agent section contains the definitions of the pod (or any other kind of agent) that will run our pipeline:

```
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          labels:
            some-label: some-label-value
        spec:
          serviceAccount: jenkins
          containers:
          - name: maven
            image: maven:3.9.9-eclipse-temurin-17
            command:
            - cat
            tty: true
          - name: busybox
            image: busybox
            command:
            - cat
            tty: true

          - name: jnlp
            image: image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-base:latest
            args: ['\${computer.jnlpmac}', '\${computer.name}']
            workDir: /tmp

        '''
      retries 2
    }
  }
``` 

Above is an example of an agent with a full pod definition, featuring 3 containers. We can also reference static pod templates rom the previous section.

### Stages

https://www.jenkins.io/doc/book/pipeline/syntax/#declarative-directives

https://www.jenkins.io/doc/book/pipeline/syntax/#sequential-stages

The stages section contains a list of stages, each stage contains a list of steps, that runs in a specific container:

```
stages {
    stage('Inspect Containers') {
      steps {
        container('maven') {
            sh "mvn --version"
            sh "java -version"
            sh "env"
            sh "ls"
        }
        container('busybox') {
          sh 'env'
        }
      }
    }
    stage('Run Unit Tests') {
      steps {
        container('maven') {
            sh "mvn test -Dmaven.repo.local=$WORKSPACE/.m2/repository"
            sh "rm -r $WORKSPACE/.m2/repository"
        }
  
      }
    }
    stage('Trigger docker build') {
      steps {
        container("jnlp") {
            sh "ls -al"
            sh "oc start-build -F java-api --from-dir=`pwd`"
          }
      }
    }
  }
```

Inside each steps section we can define different containers section and the steps that must run on each container.

During the training we only used the step kind `sh`, which runs a command. Different plugins provides different step kinds that can be used inour Jenkinsfile.

The Jenkins pipeline automatically handles the code checkout from our repository, and the connection between the pod agent and the server.

Find the full example run during the training in the `axpe-java-api` repository: https://github.com/nachomillangarcia/axpe-java-api. This is a self-contained Jenkinsfile that includes the agent and stages definitions, so it can run on any Jenkins installation with the Kubernetes plugin.

## Jenkins Pipeline Jobs

https://www.jenkins.io/doc/tutorials/build-a-multibranch-pipeline-project/#create-your-multibranch-pipeline-project

To start building a Jenkinsfile pipeline in Jenkins, we must create a new job of the kind `Pipeline` or `Multibranch Pipeline`.

In a Jenkins job of those kinds, we just define the repository that contains the Jenkinspipeline to run. We must add credentials to connect to private repositories or avoid rate limiting from public GitHub.

The Multibranch pipeline is especially useful, as it discovers all the branches and PRs (we can put some restrictions), and automatically creates sub-jobs that runs the Jenkinsfile on each branch.

We can also configure automatic triggers (push, PR creation, etc). And a polling interval for Jenkins to scan changes in the repository, so jobs can trigger automatically on those events. We can always launch a run mannually from the web console.

Once it's all set up, we can see and trigger runs of our new job. For each run we can explore logs for every step, build history, or artifacts.


# Tekton Pipelines

https://tekton.dev/docs/

Tekton is a modern CI/CD system made with Kubernetes architecture in mind. It defines different Kuebernetes objects that define pipelines to build our projects. A Tekton server is in charge of managing those objects and run the pods with the desired scripts. It also include systems to manage automatic triggers from Git repositories.

## Installation

During the training we used Tekton pipelines in Openshift. Openshift has its own flavor of Tekton, called Openshift pipelines: https://www.redhat.com/es/technologies/cloud-computing/openshift/pipelines   OPenshift pipelines are usually included in any Openshift installation.

We can use the same objects in any Kubernetes cluster, installing the Tekton operator via Helm chart. There are two different components:

* Tekton pipelines: includes the resources needed to run the tasks and pipelines: https://tekton.dev/docs/installation/pipelines/
* Tekton triggers: the resources needed to configure automatic triggers for pipelines: https://tekton.dev/docs/installation/triggers/

## Tekton Pipeline Object reference

https://tekton.dev/docs/pipelines/


### Task

https://tekton.dev/docs/pipelines/tasks/

A Task is a collection of Steps that you define and arrange in a specific order of execution as part of your continuous integration flow. A Task executes as a Pod on your Kubernetes cluster. A Task is available within a specific namespace, while a cluster resolver can be used to access Tasks across the entire cluster.

```
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: maven-unit-test
spec:
  workspaces:
  - name: source
  steps:
    - name: unit-test
      image: maven:3.9.9-eclipse-temurin-17
      workingDir: /workspace/source
      script: |
        #!/bin/bash
        mvn --version
        java -version
        env
        ls
        mvn test -Dmaven.repo.local=/workspace/source/.m2/repository
        rm -r /workspace/source/.m2/repository
```

The example above is a task that runs `mvn test` in a specific docker image

#### Tasks catalog

There are different catalogs with pre-defined tasks that can be reused.

Openshift pipelines include a bunch of tasks in the namespace `openshift-pipelines`. Those can be explored with the command `kubectl -n openshift-pipelines get tasks`

There is also a public catalog with task definition to be reused in your projects: https://github.com/tektoncd/catalog

### TaskRun

https://tekton.dev/docs/pipelines/taskruns/

A TaskRun allows you to instantiate and execute a Task on-cluster.  A TaskRun executes the Steps in the Task in the order they are specified until all Steps have executed successfully or a failure occurs

### Pipeline

https://tekton.dev/docs/pipelines/pipelines/

A Pipeline is a collection of Tasks that you define and arrange in a specific order of execution as part of your continuous integration flow. A pipleine can include different conditions to customize how the tasks run, as parameters, workspace, etc.

```
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: java-api-build
spec:

  workspaces:
  - name: workspace

  params:
  - name: git-url
    type: string
    description: Git https url
    default: https://github.com/nachomillangarcia/axpe-java-api.git
  - name: git-branch
    type: string
    description: Git branch
    default: main
  - name: imageStream
    type: string
    description: imageStream
    default: java-api


  tasks:
  - name: fetch-repository
    taskRef:
      resolver: cluster
      params:
      - name: kind
        value: task
      - name: name
        value: git-clone
      - name: namespace
        value: openshift-pipelines
    workspaces:
    - name: output
      workspace: workspace
    params:
    - name: URL
      value: $(params.git-url)
    - name: SUBDIRECTORY
      value: ""
    - name: DELETE_EXISTING
      value: "true"
    - name: REVISION
      value: $(params.git-branch)

  - name: inspect-workspace
    taskRef:
      name: inspect-workspace
    workspaces:
    - name: source
      workspace: workspace    
    runAfter:
    - fetch-repository

  - name: maven-unit-test
    taskRef:
      name: maven-unit-test
    workspaces:
    - name: source
      workspace: workspace    
    runAfter:
    - fetch-repository 

  - name: build-image
    taskRef:
      resolver: cluster
      params:
      - name: kind
        value: task
      - name: name
        value: buildah
      - name: namespace
        value: openshift-pipelines
    params:
    - name: IMAGE
      value: image-registry.openshift-image-registry.svc:5000/$(context.pipelineRun.namespace)/$(params.imageStream)
    workspaces:
    - name: source
      workspace: workspace
    runAfter:
    - maven-unit-test
```

The example above includes different params to customize the execution of 4 tasks, two of them included with openshift pipelines (`fetch-repository` and `build-image`), and two defined by ourselves (`inspect-workspace` and `maven-unit-test` )


### PipelineRun

A PipelineRun allows you to instantiate and execute a Pipeline on-cluster. PipelineRun automatically creates corresponding TaskRuns for every Task in your Pipeline.

## Tekton Triggers object reference

https://tekton.dev/docs/triggers/

### Trigger Template

https://tekton.dev/docs/triggers/triggertemplates/


A TriggerTemplate is a resource that specifies a blueprint for the resource, such as a TaskRun or PipelineRun, that you want to instantiate and/or execute when your EventListener detects an event. It exposes parameters that you can use anywhere within your resource’s template.

```
apiVersion: triggers.tekton.dev/v1beta1
kind: TriggerTemplate
metadata:
  name: java-api-build
spec:
  params:
  - name: git-name
    description: Git repo name
  - name: git-url
    description: Git https url
  - name: git-branch
    description: Git branch
    default: main


  resourcetemplates:
  - apiVersion: tekton.dev/v1
    kind: PipelineRun
    metadata:
      generateName: build-deploy-$(tt.params.git-name)-
    spec:
      taskRunTemplate:
        serviceAccountName: pipeline
      pipelineRef:
        name: java-api-build
      params:
      - name: git-url
        value: $(tt.params.git-url)
      - name: git-branch
        value: $(tt.params.git-branch)
      - name: imageStream
        value: $(tt.params.git-name)
      workspaces:
      - name: workspace
        volumeClaimTemplate:
          spec:
            accessModes:
              - ReadWriteOnce
            resources:
              requests:
                storage: 500Mi
```

The example above creates a PipelineRun from our pipeline, with its configured parameters and a volumeClaimTemplate for persistence.

### Trigger Binding

A TriggerBinding allows you to extract fields from an event payload and bind them to named parameters that can then be used in a TriggerTemplate. For instance, one can extract the commit SHA from an incoming event and pass it on to create a TaskRun that clones a repo at that particular commit.

```
apiVersion: triggers.tekton.dev/v1beta1
kind: TriggerBinding
metadata:
  name: java-api-build
spec:
  params:
  - name: git-url
    value: $(body.repository.clone_url)
  - name: git-name
    value: $(body.repository.name)
  - name: git-branch
    value: $(body.head_commit.id)
```

### Trigger

https://tekton.dev/docs/triggers/triggers/

A Trigger specifies what happens when the EventListener detects an event. A Trigger specifies a TriggerTemplate, a TriggerBinding, and optionally an Interceptor.

```
apiVersion: triggers.tekton.dev/v1beta1
kind: Trigger
metadata:
  name: java-api-build
spec:
  serviceAccountName: pipeline
  interceptors:
    - ref:
        name: "github"
      params:
        - name: "secretRef"
          value:
            secretName: webhook-secret
            secretKey: token
        - name: "eventTypes"
          value: ["push"]
  bindings:
    - ref: java-api-build
  template:
    ref: java-api-build
```

### Event Listener

Finally, an EventListener is a Kubernetes object that listens for events at a specified port on your Kubernetes cluster. It exposes an addressable sink that receives incoming event and specifies one or more Triggers. The sink is a Kubernetes service running the sink logic inside a dedicated Pod.

```

apiVersion: triggers.tekton.dev/v1beta1
kind: EventListener
metadata:
  name: java-api-build
spec:
  serviceAccountName: pipeline
  triggers:
    - triggerRef: java-api-build
```

Once we have configured all the YAMLs of this section, we have a pod created by the EventListener that we can expose and configure GitHub webhooks to forward repository events to that address. For every request received by the event listener pod, it will match the requests agains all triggers. If a trigger is configured for that event, it will create the corresponding PipelineRun, based on the Trigger Template and Binding configuration.

With all the above we cna have an automatic and reusable CI/CD pipeline for our application on GitHub.

# ArgoCD

ArgoCD is a Continuous Delivery platform for Kubernetes. It allows to deploy Kubernetes YAMLs stored in Git or Helm easily, with features to access all kind of repositories and sync status to Kubernetes cluster.

### Installing

```
helm repo add argo https://argoproj.github.io/argo-helm
helm install argocd argo/argo-cd --namespace argocd --create-namespace --values argocd/argocd-values.yaml
```

Now we install the CLI to manage ArgoCD from the terminal

```
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

For the initial login, we need to reset admin password using the CLI:

```
argocd admin initial-password -n argocd
argocd login <ARGOCD SERVER IP>
argocd account update-password
```

Now we can access the web UI and log in with the admin user

### Projects

https://argo-cd.readthedocs.io/en/stable/user-guide/projects/

Projects provide a logical grouping of applications, which is useful when Argo CD is used by multiple teams.

They allow restricting namespaces, resources and sources for different teams and applications

```
apiVersion: argoproj.io/v1alpha1
kind: AppProject
metadata:
  name: axpe
  namespace: argocd
  # Finalizer that ensures that project is not deleted until it is not referenced by any application
  finalizers:
    - resources-finalizer.argocd.argoproj.io
spec:
  description: Axpe CD Project
  sourceRepos:
  - https://github.com/nachomillangarcia/axpe-java-api.git
  - https://github.com/nachomillangarcia/formadoresit-axpe.git
  - ghcr.io/nachomillangarcia/axpe-java-api
  destinations:
  - namespace: default
    server: https://kubernetes.default.svc

  clusterResourceWhitelist:
  - group: ''
    kind: Namespace

  namespaceResourceBlacklist:
  - group: ''
    kind: ResourceQuota
  - group: ''
    kind: LimitRange
  - group: ''
    kind: NetworkPolicy
```

### Applications

https://argo-cd.readthedocs.io/en/stable/

Application is the main object for ArgoCD. It represents the YAMLs to be deployed, which should be stored in a Git or Helm repository. Application manages sinchronization between the source and the deployed resource in our cluster.

Example of Helm Chart application, which deploys a chart from the GitHub OCI repository, with customised values, in the default namespace:

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: chart-java-api
  namespace: argocd
spec:
  project: axpe
  source:
    repoURL: ghcr.io/nachomillangarcia/axpe-java-api
    chart: chart-java-api
    targetRevision: "*"
    helm:
      releaseName: argocd-chart-java-api
      valuesObject:
        nodePort: 32082
  destination:
    name: "in-cluster"
    namespace: default
  syncPolicy:
    automated:
      enabled: true
      selfHeal: true
    syncOptions:
      - ServerSideApply=true           # forces SSA
      - ApplyOutOfSyncOnly=true        # optional: faster syncs
```

Example of a Git source application. This one also deployes a helm chart but it's stored directly in Git. We add a secondary source to point to the values stored in a different path in the same repository:

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: github-java-api
  namespace: argocd
spec:
  project: axpe
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  sources:
  - repoURL: https://github.com/nachomillangarcia/formadoresit-axpe.git
    path: helm/java-api
    targetRevision: HEAD
    helm:
      valueFiles:
      - $values-repo/helm/java-api-production.yaml

  - repoURL: https://github.com/nachomillangarcia/formadoresit-axpe.git
    targetRevision: HEAD
    ref: values-repo
    
  syncPolicy:
    automated:
      enabled: false
      selfHeal: true
    syncOptions:
      - ServerSideApply=true           # forces SSA
      - ApplyOutOfSyncOnly=true        # optional: faster syncs
```

We added automated sync policy so it whet's synced everytime it detects a change in the source

# Next steps

## Networking projects

### Gateway API

Gateway API is a project for Kubernetes that allows exposing all the service in a cluster to the Internet, using a single entrypoint called Gateway. The Gateway then routes each request to the matching service using rules we can configure. 

https://gateway-api.sigs.k8s.io/

It's a replacement for the old Ingress API for Kubernetes. 

Openshift has its own system to achieve that, called routes: https://docs.redhat.com/en/documentation/openshift_container_platform/4.11/html/networking/configuring-routes

### External DNS

https://github.com/kubernetes-sigs/external-dns

External DNS is a side Kubernetes project that automatically configure DNS providers to point to Kubernetes services and Gateways. It integrates with the Gateway API so it can configure the DNS domains defined in Gateway rules, to point to th external IP of our Gateway.

### Cert Manager

https://cert-manager.io/

Same external-dns do for DNS entries, Cert Manager do for TLS certificates. It integrates with different providers to generate TLS certificates for the domains configured in the Gateway rules.

## Dashboards

- Headlamp: https://headlamp.dev/  Local and web, multi-cluster support. It's the new default for Kubernetes dashboards
- Lens IDE (open source): https://github.com/lensapp/lens  Local IDE to manage Kubernetes clusters
- Plugins for your trusted IDE. All the most popular IDEs support Kubernetes via plugins. VS Code has a really good one https://code.visualstudio.com/docs/azure/kubernetes


Thanks for reaching the end, I wish you luck on your Kubernetes journey :)

