# k8s-autoscale-demo

Kubernetes Autoscaling Demo

***Reference: From Nginx Microservices March 2022: Kubernetes Networking***

## Tools/Components Involved

- Minikube - K8s Utility
- Kubectl - K8s CLI
- Deployment - K8s Component
- Helm (Package Manager) - Open Source
- Ingress rules and Ingress Controller (Nginx Ingress Controller) - Open Source
- Locust - Open Source
- Prometheus - Open Source
- Horizontal Pod Autoscaling (HPA) - K8s Component
- Kubernetes Event-Driven Autoscaler (KEDA) - Open Source

## Demo Summary

1. Create single K8s cluster locally using minikube
2. Exposing the app to external via Nginx Ingress Controller
3. Setup Prometheus server to collect metrics
4. Run load testing using Locust
5. Implement KEDA to forward the metrics from Prometheus server to HPA
6. HPA will do the scaling out of Nginx Ingress Controller to handle to high traffic volumes

## Demo

### 1. Create the Cluster

``` minikube start ```

### 2. Install the App

- Create a Deployment (deployment-1.yaml)
- Deploy PodInfo App

``` kubectl apply -f deployment-1.yaml ```

- Confirm App Deployment

``` kubectl get pods ```

### 3. Install Nginx Ingress Controller using Helm

``` helm repo add nginx-stable https://helm.nginx.com/stable ```

```

    helm install main nginx-stable/nginx-ingress \
  --set controller.watchIngressWithoutClass=true
  --set controller.service.type=NodePort \
  --set controller.service.httpPort.nodePort=30005 

```

- Verify Nginx Ingress Controller is running

``` kubectl get pods ```

### 4. Route Traffic to your App

- Create an Ingress Manifest (ingress-1.yaml)
- Submit the ingress manifest

``` kubectl apply -f 2-ingress.yaml ```

### 5. Explore Metrics

- Explore Available Metrics <https://github.com/nginxinc/nginx-prometheus-exporter#exported-metrics> 

| Name                         | Type    | Description                           |
| :---                         | :---:   | :---                                  |
|`nginx_connections_accepted`  | Counter | Accepted client connections           |
|`nginx_connections_active`    | Gauge   | Active client connections             |
|`nginx_connections_handled`   | Counter | Handled client connections            |
|`nginx_connections_reading`   | Gauge   | Connections where NGINX is reading the request header |
|`nginx_connections_waiting`   | Gauge   | Idle client connections               |
|`nginx_connections_writing`   | Gauge   | Connections where NGINX is writing the response back to the client |
|`nginx_http_requests_total`   | Counter | Total http requests                   |

