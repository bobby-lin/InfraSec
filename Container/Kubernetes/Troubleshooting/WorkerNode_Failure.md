# Debugging Workers Nodes Failures

Check Node Status
- Check the Conditions status:
```
kubectl get nodes
kubectl describe node worker-1
```
Check for memory and disk space
```
top
df -h
```
Check Kubelet status in the worker's node
```
service kubelet status
systemctl status kubelet
sudo journalctl -u kubelet
```
View logs
```
journalctl -u kubelet -f
```
Check Certificates
- Correct Group?
- Issued by CA?
```
openssl x509 -in /var/lib/kubelet/worker-1.crt -text
```
Restart the kubectl
```
systemctl start kubelet
```
## Fixing issues
Edit the kubelet config file in worker node
```
nano /var/lib/kubelet/config.yaml
```

## Examples
If the kubelet in the worker's node is dead
```
systemctl status kubelet
[...]
Active: inactive (dead)
```
Wrong ports to kube-apiserver in kublet configuration
```
kubelet_node_status.go:93] Unable to register node "node01" with API server: Post "https://controlplane:6553/api/v1/nodes": dial tcp 10.44.255.8:655
```
```
# Change the port
nano /etc/kubernetes/kubelet.conf
# Restart
systemctl restart kubelet
```