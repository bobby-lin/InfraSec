# Debugging Control Plane
https://kubernetes.io/docs/tasks/debug-application-cluster/debug-cluster/

Check Nodes
```
kubectl get nodes
```
Check pod status
```
kubectl get pods
```
Check Controlplane pods
```
kubectl get pods -n kube-system
```
Check Controlplane servces (if deployed as services)
```
service kube-apiserver status
service kube-controller-manager status
service kube-scheduler status
service kubelet status
service kube-proxy status
```
Check Service Logs
```
kubectl logs kube-apiserver-master -n kube-system
```
```
sudo journalctl -u kube-apiserver
```
## Fixing issues
Check the manifests under
```
/etc/kubernetes/manifests
```

## Examples
Investigate the pods which are crashed.
```
kubectl describe pods -n kube-system kube-xxxxxxx
cat /etc/kubernetes/manifests/xxxxx.yaml
```
Wrong config files provided
```
kubectl logs -n kube-system kube-controller-manager-controlplane
I1010 10:23:48.177846       1 serving.go:331] Generated self-signed cert in-memory
stat /etc/kubernetes/controller-manager-XXXX.conf: no such file or directory
```
File exists. But Filepath is not mounted
```
unable to load client CA file "/etc/kubernetes/pki/ca.crt": open /etc/kubernetes/pki/ca.crt: no such file or directory
```