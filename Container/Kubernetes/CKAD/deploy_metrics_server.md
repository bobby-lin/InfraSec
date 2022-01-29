# Deployment
```
git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git
cd kubernetes-metrics-server
kubectl create -f .
```
## Check metrics for nodes, pods, etc.
```
kubectl top node
kubectl top pods
```