# Operation
This repository contains all deployment files for the application.

## Getting Started
To start the application locally, make sure you have Docker and Docker Compose installed, then run:
`docker-compose -f services.yml -f deployments.yml up`
This will start the entire application stack locally.

## Deployment
*Kubernetes instructions will be added later* 🚧

To deploy the application to a Kubernetes cluster, use the deployment file provided in this repository. 
For example, to deploy to Kubernetes, you can use the `kubectl apply` command to apply the deployment files:
`kubectl apply -f deployment.yml`

## Codebase Overview
- `docker-compose.yml`: Defines the services that make up the application stack.
- `README.md`: Provides documentation on how to start the application and deploy it to a cloud provider or Kubernetes cluster.