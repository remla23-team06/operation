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

## Kubernetes

### Requirements

Make sure that istio has been downloaded and installed in your cluster using `istioctl install`.

Then, install the following tools:
- Grafana: `kubectl apply -f <istio-instal-dir>/samples/addons/grafana.yaml`
- Prometheus: `kubectl apply -f <istio-instal-dir>/samples/addons/prometheus.yaml`
- Jaeger: `kubectl apply -f <istio-instal-dir>/samples/addons/jaeger.yaml`
- Kiali: `kubectl apply -f <istio-instal-dir>/samples/addons/kiali.yaml`

_**NOTE**_: In the steps above, replace `<istio-instal-dir>` with the path to the directory where you have installed `istioctl` (e.g: `$HOME/istio-1.17.2`).

### Deployment

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
