# Taint and Toleration
https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/

To restrict whether a node will accept a pod but doesn't guarantee
- Taints are configured on nodes
- Tolerations are configured on pods

## Useful commands
Creating a Taint in a node
```
kubectl taint nodes node-name key=value:[taint-effect] # where taint-effect = {NoSchedule, PreferNoSchedule, NoExecute}

kubectl taint nodes node001 app=blue:NoSchedule 
```
Creating a Toleration in pod
```yaml
apiVersion:
kind: Pod
metadata:
  name: myapp
spec:
  containers:
  - name : nginx
    image: nginx
  tolerations:
  - key: "app"
    operator: "Equal"
    value: "blue"
    effect: "NoSchedule"
```
Check if a node contains a `Taint`?
```
kubectl describe node node01 | grep Taint
```
We can see that the master node is configured not to accept other created Pods
```
kubectl describe nodes kubemaster | grep Taint
```
Remove a tain from node (notice the `-` sign)
```
kubectl taint nodes controlplane node-role.kubernetes.io/master:NoSchedule-
```

## Common errors
The pod is created but in `Pending` because it cannot tolerate the existing nodes
```
root@controlplane:~# kubectl get pods
NAME       READY   STATUS    RESTARTS   AGE
nginx-app   0/1     Pending   0          13s
```