# Operation
This repository contains all deployment files for the application.

## Codebase Overview
- `docker-compose.yml`: Defines the services that make up the application stack.
- `README.md`: Provides documentation on how to start the application and deploy it to a cloud provider or Kubernetes cluster.
- `deployment.yml`: Defines the Kubernetes services with Deployments, Services and Ingress.
- `grafana.json`: Defines all the necessary graphs for the Grafana visualization tool.

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

## Enable Prometheus for Monitoring
Add the Prometheus stack to the Kubernetes cluster
```
helm repo add prom-repo https://prometheus-community.github.io/helm-charts
```

Update the Helm repositories
```
helm repo update
```

Install the Prometheus stack:
```
helm install myprom prom-repo/kube-prometheus-stack
```

Run service for monitoring:
```
minikube service myprom-kube-prometheus-sta-prometheus --url
```

The `deployment.yml` manifest includes the `ServiceMonitor` custom resource.

## Deployment

To deploy the application to a Kubernetes cluster, run:
```
kubectl apply -f deployment.yml
```

To tunnel the Ingress to `localhost`, run:

```
minikube tunnel
```

To access the web-page of the application, open your browser and go to: 
```
http://localhost
```
