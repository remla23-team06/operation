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

_**NOTE**_: In the steps above, replace `<istio-instal-dir>` with the path to the directory where you have installed `istioctl` (e.g: `$HOME/istio-1.17.2`).

### Optional: Volumes and Models (for manual deployment)
If you want to use your own machine learning models in the application, you can use mounted `Volumes` in the `model-service` in order to overwrite the default ones. Here's what you need to do:
1. Create a directory: `mkdir /path/to/my/models`
2. In the created directory, place your BoW sentiment model and your Classifier sentiment model, named `c1_BoW_Sentiment_Model.pkl` and `c2_Classifier_Sentiment_Model` respectively.
3. Update the `spec.template.spec` field of the `model-service` Deployment in the `manual-deployment.yml` file with:
    ```
    spec:
      containers:
      - name: model-service
        image: ghcr.io/remla23-team06/model-service:latest
        imagePullPolicy: Always
        ports:
        - containerPort: 8000
        volumeMounts:
          - name: model-volume
            mountPath: /models
      volumes:
        - name: model-volume
          hostPath:
            path: /path/to/my/models
    ```


### Deployment Option 1: Use Kubernetes deployment files
To manually deploy the application to a Kubernetes cluster, run:
```
kubectl apply -f manual-deployment.yml
```

### Deployment Option 2: Use Helm chart (recommended)
To deploy the application to a Kubernetes cluster, run:
```
helm install sentiment-chart oci://ghcr.io/remla23-team06/operation/sentiment-chart
```

### Tunnel the connection
To tunnel the Ingress to `localhost`, run:
```
minikube tunnel
```

### Access the deployed Web-App
To access the web-page of the application, open your browser and go to: 
```
http://localhost
```

If you're using Linux and you're having trouble getting the above to work, then run:
```bash
minikube service istio-ingressgateway -n istio-system
```
This will show a number of different ports for the Istio `Ingress Gateway`. The output should look something like tihs:
```
|--------------|----------------------|-------------------|---------------------------|
|  NAMESPACE   |         NAME         |    TARGET PORT    |            URL            |
|--------------|----------------------|-------------------|---------------------------|
| istio-system | istio-ingressgateway | status-port/15021 | http://192.168.49.2:30693 |
|              |                      | http2/80          | http://192.168.49.2:31005 |
|              |                      | https/443         | http://192.168.49.2:31659 |
|--------------|----------------------|-------------------|---------------------------|
[istio-system istio-ingressgateway status-port/15021
http2/80
https/443 http://192.168.49.2:30693
http://192.168.49.2:31005
http://192.168.49.2:31659]
```

You should copy and paste the URL that corresponds to target port: `http2/80`. In the example above, that is: `http://192.168.49.2:31005`.

**_NOTE_**: Don't use the URL from the above example as is. The _host_ and _port_ displayed on your system will most likely **be different**.
