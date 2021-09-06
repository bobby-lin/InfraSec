# Manual Scheduling

## Example of definition file
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
  nodeName: node01 # this is where we specify the node
```

## Useful command
Get the available nodes
```
kubectl get node
```
View the node of the pods
```
kubectl get pods -o wide
```