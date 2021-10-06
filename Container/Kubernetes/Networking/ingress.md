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