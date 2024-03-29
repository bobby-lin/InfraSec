### Installation
- Docker for Desktop
- Minikubes

### Inside a basic Pod definition yaml file for K8s
A YAMl file should consist of the following:
```yaml
apiVersion: v1 # Specify K8s api version
kind: Pod # Pod, Service, ReplicaSet, Deployment
metadata:
    name: xxxxxx
    labels: # Specify the (custom) key-value pairs
        app: xxxxxx
spec:
    containers: # List of containers in the Pod
      - name: nginx-container
        image: nginx
      - name: xxxxxx # another container
        image: xxxxxx
```

### Commands
Check version of kube command line tool
```
kubectl version
```

Create a Pod (< v1.18)
```
kubectl run my-nginx --image nginx
```

Create a Pod and Deployment (> v1.18)
```
kubectl create deployment nginx --image nginx
kubectl create deployment my-apache --image httpd
```

Get info on Pods
```
kubectl get pods
```
Delete all pods
```
kubectl delete pods --all
```

List the requested object(s) across all namespaces
```
kubectl get all
```
Delete a deployment
```
kubectl delete deployment my-apache
```
Execute command in the pod
```
kubectl exec webapp -- cat /log/app.log
```


#### Scaling Replicas
https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

Scale up a pod replicas to 2. Notice that in deployment.apps, there will be 2 pods ready
```
kubectl scale deploy/my-apache --replicas 2
```

#### Inspecting Deployment Objects

Inspecting logs
```
kubectl logs deployment/my-apache
kubectl logs deployment/my-apache --follow --tail 1
```

Inspecting logs based on deployment label
```
kubectl logs -l run=my-apache
```

Get details about an object (including events)
```
kubectl describe pod/my-apache-7b68fdd849-66h5c
```
Watch the effect of command
```
kubectl get pods -w
```
Delete a pod
```
kubectl delete pod/my-apache-xxxxxx-yyyy
kubectl delete pod/my-apache-7b68fdd849-svtv6
```
Watch the pod will be created.

Which nodes are these pods placed on?
```
kubectl get pods -o wide
```
Look at the node column

Meaning of `READY` column in the output of the `kubectl get pods` command
```
Total Container Running / Total Containers
```