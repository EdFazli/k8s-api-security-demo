# k8s-api-security-demo

Kubernetes API Protection using Rate Limiting Demo

***Reference: From Nginx Microservices March 2022: Kubernetes Networking***

## Tools/Components Involved

- Nginx Ingress Controller
- Locust
- Helm
- Virtual Server Manifest

## Demo Summary

1. Flood the web and API server with high traffic
2. Deploy 2 Nginx Ingress Controllers
3. Implement rate limiting on the Nginx Ingress Controller serving the API

## Demo

### 1. Create the cluster

``` minikube start ```

### 2. Install Nginx Ingress Controller

- Create new namespace name nginx

``` kubectl create namespace nginx ```

- Add nginx repo and download stable version

``` helm repo add nginx-stable https://helm.nginx.com/stable ```

```
  helm install main nginx-stable/nginx-ingress \
  --set controller.watchIngressWithoutClass=true \
  --set controller.ingressClass=nginx \
  --set controller.service.type=NodePort \
  --set controller.service.httpPort.nodePort=30010 \
  --set controller.enablePreviewPolicies=true \
  --namespace nginx
```

- Verify the controller is running

``` kubectl get pod --namespace nginx ```

### 3. Create Ingress Manifest

