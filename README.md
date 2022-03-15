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