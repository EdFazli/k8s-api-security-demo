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

- Create App and API deployment (deployment.yaml) and then apply

``` kubectl apply -f deployment.yaml ```

- Verify the deployment

``` kubectl get pods ```

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

- Create ingress manifest (ingress-nginx.yaml) and then apply

``` kubectl apply -f ingress-nginx.yaml ```

### 4. Test the Ingress Configuration

- To see the traffic is routed correctly to the API

``` kubectl get service --namespace nginx ```

- Launch temporary busybox

``` kubectl run -ti --rm=true busybox --image=busybox ```

- Issue request to Nginx Ingress Controller with hostname "api.example.com"

``` wget --header="Host: api.example.com" -qO- main-nginx-ingress.nginx ```

- Issue request to Nginx Ingress Controller with hostname "example.com" to simulate web browser

``` wget --header="Host: example.com" --header="User-Agent: Mozilla" -qO- main-nginx-ingress.nginx ```

### 5. Install Locust

- Create and Install Locust (locust.yaml)

``` kubectl apply -f locust.yaml ```

### 6. Simulate Traffic with Locust

> Host: http://main-nginx-ingress.nginx

### 7. Deploy 2 New Ingress Controllers

- First delete the current ingress deployment

``` kubectl delete -f ingress-nginx.yaml ```

- Create 2 new namespaces

``` kubectl create namespace nginx-web ```

``` kubectl create namespace nginx-api ```

### 8. Install Web Namespace Ingress Controller

```
    helm install web nginx-stable/nginx-ingress --set controller.ingressClass=nginx-web \
  --set controller.service.type=NodePort \
  --set controller.service.httpPort.nodePort=30020 \
  --namespace nginx-web
```

- Create and apply web manifest (ingress-web.yaml)

