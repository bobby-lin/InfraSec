# Preparation Guide
https://kubernetes.io/docs/reference/kubectl/conventions/

## Tedious to create and edit YAML files manually
### Create objects without YAML

Pods
```
kubectl run nginx --image=nginx
```
Deployment
```
kubectl create deployment --image=nginx nginx
```

### Dry-run and output the YAML files
Create Pod yaml file
```
kubectl run nginx --image=nginx --dry-run=client -o yaml
```
Create Deployment yaml file
```
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
```
Save Deployment yaml file
```
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml
```
Use the saved yaml file
```
kubectl create -f nginx-deployment.yaml
```
For k8s version 1.19+, use `--replicas` option to create deployment with 4 replicas
```
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
```