# Preparation Guide
- https://kubernetes.io/docs/reference/kubectl/conventions/
- https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands


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
Expose a port in a Pod
```
kubectl run custom-nginx --image=nginx --port=8080 -o yaml > custom-nginx.yaml
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
Create a Service - Nodeport and expose the pod's port on nodeport of the nodes
A) This can use the pod label as selectors but cannot specify the node port
```
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml
```
B) If we use this command, then pods labels cannot be used as selectors
```
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
```
Most of the time, using the pod label as selectors are useful. So try to use option A to create a service. If you want to change the node-port, then edit the service definition file to add the `nodePort`.kubec