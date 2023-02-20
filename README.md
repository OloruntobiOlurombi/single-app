# single-app

An application can be created in Argo CD from the UI, CLI, or by writing a Kubernetes manifest that can then be passed to kubectl to create resources.

Creating an ArgoCD application in the UI

First, navigate to the +NEW APP on the left-hand side of the UI. 

Next, add the following to create the application:

General Section:


Application Name: TBD

Project: default

Sync Policy: Automatic

Source Section:

Repository URL/Git: this is the GitHub repository URL

Branches: main

Path: TBD

Destination Section:


Cluster URL: select the cluster URL you are using

Namespace: default

Then, click CREATE and you have now created your Argo CD application.

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
