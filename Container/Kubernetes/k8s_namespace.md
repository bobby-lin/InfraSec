# K8s Namespace
https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

A `default` namespace is used for objects that are created without other namespace.

## Namespace and DNS
You can connect to a specific object with this DNS entry format:
```
<service-name>.<namespace-name>.svc.cluster.local
db-service.marketing.svc.cluster.local
```

## Creating a namespace
```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```
## Example of a specified namespace in a definition file
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
spec:
  containers:
    - name: myapp
      image: nginx
      resources:
        limits:
          memory: "128Mi"
          cpu: "500m"
```

## Useful commands
Create a namespace
```
kubectl create namespace dev
```
View namespace
```
kubectl get namespace
```
Create or view objects in a specific namespace
```
kubectl run nginx --image=nginx --namespace=<insert-namespace-name-here>
kubectl get pods --namespace=<insert-namespace-name-here>
```
Setting namespace
```
kubectl config set-context --current --namespace=<insert-namespace-name-here>
# Validate it
kubectl config view --minify | grep namespace:
```

## Errors
This means the Host Name that you are connecting is wrong. Check if the namespace is correct.
```
[Errno -2] Name does not resolve
```