# Daemon sets
Daemon set is used to create at least one copy of a Pod in each new node.

Use cases:
- Adding Monitoring agent to each node
- Adding Logger agent to each node
- `kube-proxy` is a good example

## Example of Daemon set def. file
It is similar to ReplicaSet def. file except for the `kind` field.
```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: monitoring-daemon
  namespace: kube-system
spec:
  selector:
    matchLabels:
      app: monitoring-agent
  template:
    metadata:
      labels:
        app: monitoring-agent
    spec:
      containers:
      - name: monitoring-agent
        image: monitoring-agent:latest
```

## Useful commands
View the daemonset
```
kubectl get daemonset
```