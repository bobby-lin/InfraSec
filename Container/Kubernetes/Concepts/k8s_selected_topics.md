## Storage
##### 2 types of Volume
Volumes: 
- Volume is tied to lifecycle of a Pod.
- All containers in a Pod can share the volume.

PersistentVolumes:
- Created at Cluster level and outlives a Pod.
- Separates storage config from Pod using it.
- Multiple Pods can share the persistent volumes.

Container Storage Interface (CSI)
https://kubernetes.io/blog/2019/01/15/container-storage-interface-ga/
- This is a standard for connecting storage system to Kubernetes

## Ingress
Problem: How do we route outside connections based on hostname or URL?
- Ingress controllers can do this with 3rd party proxy.
- Popular 3rd party proxies include NGINX, Traefix, HAProxy, F5, Istio, Envoy etc.
- Dev and Ops team need to label the resources so that Ingress controller can use the label
- Try NGINX or Traefix and Let's Encrypt

## Custom Resource Definition (CRD)
- For adding 3rd party resources and controllers which extends k8s API and CLI.
- Operator is used to automate deployment and management of complex apps. It is difficult to understand the exact steps required for complex apps. Hence Operator can be used. For example, Databases, monitoring tools, Backups, and custom ingresses.

## Helm
Problem: How do you deploy an application to k8s?

- Helm is package manager for Kubernetes
- It helps to manage k8s applications.

Note: Alternative tools can include Docker Compose on Kubernetes

## k8s Dashboard
https://github.com/kubernetes/dashboard
https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/creating-sample-user.md
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.2.0/aio/deploy/recommended.yaml
kubectl proxy
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
```

## Namespaces
- Namespaces are used to limit scope (virtual clusters)
- Note that they are not related to Docker / Linux namespaces
```
kubectl get namespaces
kubectl get all --all-namespaces
```

## Future of K8s
- Service Mesh: New layer of distributed app traffic for better control, security and monitoring
