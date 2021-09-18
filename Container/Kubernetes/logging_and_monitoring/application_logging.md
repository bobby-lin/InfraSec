# Application Logging
Important for developers to debugs issues.

How to view the logs of a running application in a pod?

Suppose we have a manifest file here:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: front-end
spec:
  containers:
    - name: nginx-container
      image: nginx
```
You create the pod here
```
kubectl create -f pod.yaml
```
Tail the logs
```
kubectl logs -f myapp-pod
```

## Multiple containers
If your pod has multiple containers running, you need to specify the additional container name.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
    type: front-end
spec:
  containers:
    - name: nginx-container
      image: nginx
    - name: elastic-container
      image: elasticsearch
```
You create the pod here
```
kubectl create -f pod.yaml
```
Tail the logs
```
kubectl logs -f myapp-pod elastic-container
```

## Common errors
There are multiple containers that we didn't specify
```
root@controlplane:~# kubectl logs webapp-2
error: a container name must be specified for pod webapp-2, choose one of: [simple-webapp db]
```