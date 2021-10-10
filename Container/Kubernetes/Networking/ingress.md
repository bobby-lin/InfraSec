# Ingress
https://kubernetes.io/docs/concepts/services-networking/ingress/

https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-ingress-em-

https://kubernetes.io/docs/concepts/services-networking/ingress/#path-types

Think of Ingress as a Layer 7 Load Balancer in Kubernetes.

Ingress can abstract these tasks:
- Routing
- SSL Configuration

## Idea
https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/

### Ingress Controller (does not deploy by default): 

Deploy a proxy such as Nginx, HAProxy, Traefik, Istio etc.

Four requirements:
- Deployment
- Service Account
- ConfigMap
- Authorization for Service Account

To view the existing ingress controller:
```
kubectl get deployment --all-namespaces
kubectl get pods --all-namespaces
```

### Ingress Resources
For k8s version 1.20+:
- Create an Ingress resource from the imperative way like this:

```
kubectl create ingress <ingress-name> --rule="host/path=service:port"
```

Example: `kubectl create ingress ingress-test --rule="wear.my-online-store.com/wear*=wear-service:80`

Rules
Paths

Traffic Patterns:
- By URL (example.com/home. example.com/welcome)
- By Hostname (a.example.com, b.example.com) 

To view Ingress resources:
```
kubectl get ingress --all-namespaces
```

Example:
Create an Ingress resource
```
kubectl create ingress ingress-wear-watch --rule="/wear=wear-service:8080" --rule="/watch=video-service:8080" -n app-space --dry-run=client -o yaml > ingress.yaml
```

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
  creationTimestamp: "2021-10-07T01:53:12Z"
  generation: 1
  name: ingress-wear-watch
  namespace: app-space
  resourceVersion: "5513"
  uid: 710981a0-508c-4904-b07d-55d6db9bde7b
spec:
  rules:
  - http:
      paths:
      - backend:
          service:
            name: wear-service
            port:
              number: 8080
        path: /wear
        pathType: Exact
      - backend:
          service:
            name: video-service
            port:
              number: 8080
        path: /watch
        pathType: Exact
status:
  loadBalancer:
    ingress:
    - {}
```