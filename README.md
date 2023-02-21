# Creation and Syncing of Kubertenes Application.

## Overview:

- In this article we will examine how to create and Sync a kubernetes application using Argo CD from the UI and CLI. An application can be created in Argo CD from the UI, CLI, or by writing a Kubernetes manifest that can then be passed to kubectl to create resources. First we will consider the meaning of Agro CD and principles guilding it.

### What is Agro CD?

- Argo CD is a declarative, GitOps continuous delivery tool for Kubernetes.

### Argo CD implements all GitOps principles that we described in the following way:

- You install Argo CD as a controller in the Kubernetes cluster. Usually you install Argo CD on the same cluster that it manages. It is also possible for Argo CD to manage external clusters.

- You store your manifests in Git. Argo CD is agnostic on the type of manifests you can use. It supports plain Kubernetes manifests, Helm charts, Kustomize definitions, and other templating mechanisms.

- You create an Argo CD application by defining which Git repository to monitor and to which cluster/namespace this application should be installed.

- From now on, Argo CD monitors the Git repository, and when there is a change, it automatically brings the cluster to the same state.

- Optionally Argo CD can deploy applications to other clusters (and not just the one on which it is installed).

<img width="991" alt="Screenshot 2023-02-20 at 4 02 33 PM" src="https://user-images.githubusercontent.com/40290711/220215334-08293a2a-3c9d-4361-b199-dc5be4cb7230.png">


### Prerequisites:
1. Basic knowledge of Kubernetes is required.
2. Install Argo CD as a controller in the Kubernetes cluster. 
3. Clone this github repo https://github.com/OloruntobiOlurombi/gitops-certification-examples

Now that we have the prerequisites are out of the way,  we will proceed with our objective.

### Steps:

### Creating an ArgoCD application in the UI:

- To start with we will have to navigate to the ***NEW APP*** on the left-hand side of the UI. 

- Next, add the following to create the application:


#### General Section:

- Application Name: simple-app

- Project: default

- Sync Policy: Automatic


<img width="1439" alt="Screenshot 2023-02-20 at 11 29 35 AM" src="https://user-images.githubusercontent.com/40290711/220214888-a2d60e11-cb33-4740-a624-24177cda82c6.png">


#### Source Section:

- Repository URL/Git: this is the GitHub repository URL above (https://github.com/OloruntobiOlurombi/gitops-certification-examples). Please use your cloned repository.

- Branches: main

- Path: ./simple-app

<img width="1121" alt="Screenshot 2023-02-20 at 11 30 17 AM" src="https://user-images.githubusercontent.com/40290711/220214911-136858d4-cf3c-4350-9f14-ef1a12c523fe.png">


#### Destination Section:

- Cluster URL: select the cluster URL you are using

- Namespace: default


<img width="1092" alt="Screenshot 2023-02-20 at 11 30 35 AM" src="https://user-images.githubusercontent.com/40290711/220215049-5f3381e3-db21-4e4f-a1d7-14ec1fd36180.png">


- Then, click CREATE and you have now created your Argo CD application

<img width="1433" alt="Screenshot 2023-02-20 at 11 30 52 AM" src="https://user-images.githubusercontent.com/40290711/220313451-d68d496f-0ccf-4a70-adb7-1b3969ed3ced.png">

<img width="1335" alt="Screenshot 2023-02-20 at 11 32 30 AM" src="https://user-images.githubusercontent.com/40290711/220313687-8ad21f5c-c560-4c2c-acff-6ca30f87532e.png">

<img width="1438" alt="Screenshot 2023-02-20 at 11 33 16 AM" src="https://user-images.githubusercontent.com/40290711/220313775-5b353f51-9724-446e-865f-d1ca2ef9e1fa.png">


Creating an Argo CD application with the argocd CLI
```
argocd app create {APP NAME} \
--project {PROJECT} \
--repo {GIT REPO} \--path {APP FOLDER} \
--dest-namespace {NAMESPACE} \
--dest-server {SERVER URL}
{APP NAME} is the name you want to give the application
{PROJECT} is the name of the project created or "default"
{GIT REPO} is the url of the git repository where the gitops config is located
{APP FOLDER} is the path to the configuration for the application in the gitops repo
{DEST NAMESPACE} is the target namespace in the cluster where the application will be deployed
{SERVER URL} is the url of the cluster where the application will be deployed. Use https://kubernetes.default.svc to reference the same cluster where Argo CD has been deployed
```

Once this completes, you can see the status and configuration of the application.




```
argocd app list
```
For a more detailed view of the application configuration, run:

```
argocd app get {APP NAME}
```

# Syncing an application

### Synchronizing an Argo CD application in the UI

Initially, the application is in OutOfSync state since the application has yet to be deployed, and no Kubernetes resources have been created.

To synchronize/deploy the Argo CD app:

1. Choose the tile and then select SYNC

This will provide you options of what you want to synchronize.

2. Select the default options and synchronize all manifests

Once it's deployed, you will see the resources deployed in the UI and a Healthy status.

<IMG>

# Synchronizing an Argo CD application with the argocd CLI.
  
Since the application is in OutOfSync status once created because it hasn’t been deployed yet, we will then sync the application with a sync command.
argocd app sync {APP NAME}
This synchronizes the application. To confirm it’s running, you can execute a kubectl command.
kubectl -n {NAMESPACE} get all
The application will have a status “Running” if synchronized successfully.
