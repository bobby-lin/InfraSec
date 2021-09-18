# Rollout updates and Rollbacks

## Deployment strategy
1) Recreate
- Destroy all pods
- Recreate all pods
- Use `kubectl describe deployment/xxx` to see that the replica set will be scaled down to 0.
> Downside: The application will be temporarily down

2) Rolling update (Default deploymen strategy)
- Destroy one pod and then recreate one pod.
- Do that for each pod.

Example: Look at the StrategyType!
```
root@controlplane:~# kubectl describe deployment frontend
Name:                   frontend
Namespace:              default
CreationTimestamp:      Sat, 18 Sep 2021 11:34:35 +0000
Labels:                 <none>
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               name=webapp
Replicas:               4 desired | 4 updated | 4 total | 4 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        20
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  name=webapp
  Containers:
   simple-webapp:
    Image:        kodekloud/webapp-color:v1
    Port:         8080/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  <none>
NewReplicaSet:   frontend-7776cb7d57 (4/4 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  3m30s  deployment-controller  Scaled up replica set frontend-7776cb7d57 to 4
  ```

## Useful commands
View the current deployment status
```
kubectl rollout status deployment/myapp
```
View the deployment history (the revision will be shown)
```
kubectl rollout history deployment/myapp
```
Applying a change to deployment file
```
kubectl apply -f deployment-definition.yml
```
Set a new image name for the existing deployment (Update the container named as 'nginx')
```
kubectl set image deployment/myapp nginx=nginx:1.9.1

# The container "simple-webapp" is updated with another image
kubectl set image deployment/frontend simple-webapp=kodekloud/webapp-color:v2
```
Observe the deployment strategy
```
kubectl describe deployment frontend
```
Change deployment strategy
```
kubectl edit deployment frontend
```

## Rollbacks
Rollback to previous revision
```
kubectl rollout undo deployment/myapp
```