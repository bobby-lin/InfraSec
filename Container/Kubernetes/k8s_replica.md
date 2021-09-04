# K8s - Replica setls
Official Documentation: https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/

Replicaset ensures that the desired number of pods are created and maintained.

## Example of replicaset definition file
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
  labels:
    app: guestbook
    tier: frontend
spec:
  # modify replicas according to your case
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: php-redis
        image: gcr.io/google_samples/gb-frontend:v3

```
Label and selector are used by replicaset to monitor the specific pods.

## Useful Commands
View the replica set information
```
kubectl describe replicaset myapp-replicaset
```
Create a replica set with a definition file
```
kubectl create -f replicaset-definition.yml
```
Get info about your replicaset
```
kubectl get replicaset
```
Delete the specific replicaset
```
kubectl delete replicaset myapp-replicaset
```
If you made changes in the definition file, you can update the file using this command:
```
kubectl replace -f replicaset-definition.yml
```
If you just wanna increase or decrease the number of replicas without changing the file, either one of the two commands can be used:
```
kubectl scale --replicas=6 -f replicaset-definition.yml
```
```
kubectl scale --replicas=6 replicaset myapp-replicaaset # name of replicaset
```
Edit a running replicaset
```
kubectl edit replicaset new-replica-set
```

## Common errors
apiVersion error - change the version to `apps/v1` instead of `v1`
```
error: unable to recognize "replicaset-definition-1.yaml": no matches for kind "ReplicaSet" in version "v1"
```
Change The label in selector to match the label in the template
```
The ReplicaSet "replicaset-2" is invalid: spec.template.metadata.labels: Invalid value: map[string]string{"tier":"nginx"}: `selector` does not match template `labels`
```
