# Monitoring Cluster Component
Some products that can perform the monitoring
- Prometheus
- Elastic
- Datadog


## Deploying metrics server manually

```
git clone https://github.com/kodekloudhub/kubernetes-metrics-server.git
cd kubernetes-metrics-server
kubectl create -f .
kubectl get pods --all-namespaces
```

## Useful commands
View node metrics (CPU or Memory consumption etc)
```
kubectl top node                
NAME           CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
controlplane   393m         0%     1226Mi          0%        
node01         58m          0%     374Mi           0%   
```
View pod metrics
```
kubectl top pod
```
Use sort by parameter to view a specific metrics
```
kubectl top pod --sort-by memory
```