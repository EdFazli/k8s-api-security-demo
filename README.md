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

