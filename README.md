# Operation
This repository contains all deployment files for the application.

## Getting Started
To start the application locally, make sure you have Docker and Docker Compose installed.
Also add an environment variable called `CR_PAT` containing your Github PAT. 
E.g., in Powershell (on Windows): 
```powershell
$env:CR_PAT=<PAT>
```
E.g., in Bash (on Linux):
```bash
export CR_PAT=<PAT>
```
Subsequently, run the following commands:

1. `echo $CR_PAT | docker login ghcr.io -u $ --password-stdin`
1. `docker compose up -d --pull always`

This will start the entire application stack locally and go to `localhost:8080` to try out the app.

## Deployment

To deploy the application to a Kubernetes cluster, use the deployment file provided in this repository. 
For example, to deploy to Kubernetes, you can use the `kubectl apply` command to apply the deployment files:

`kubectl apply -f deployment.yml`

Use `minikube` to tunnel the Ingress to `localhost` with:

`minikube tunnel`

For now: In order to be authorized to pull the container registries, you need to have access to a token:

`kubectl create secret docker-registry regcred --docker-server=https://ghcr.io --docker-username=<GITHUB_USERNAME> --docker-password=<TOKEN> --docker-email=<GITHUB_EMAIL>`

The `deployment.yml` contains `imagePullSecrets` with `regred`

## Codebase Overview
- `docker-compose.yml`: Defines the services that make up the application stack.
- `README.md`: Provides documentation on how to start the application and deploy it to a cloud provider or Kubernetes cluster.
- `deployment.yml`: Defines the Kubernetes services with Deployments, Services and Ingress.
