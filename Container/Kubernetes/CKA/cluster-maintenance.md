# Exercises

## Upgrading Kubernetes cluster

Doc: https://v1-20.docs.kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/

<details><summary>Solution</summary>
<p>

### Upgrading control plane nodes

```bash
# Upgrade kudeadm (change x to the patch version)
apt-get update && apt-get install -y --allow-change-held-packages kubeadm=1.20.x-00

# Verify upgrade plan
kubeadm version && kubeadm upgrade plan

# replace x with the patch version you picked for this upgrade (this will take a while)
sudo kubeadm upgrade apply v1.20.x

# Upgrade kubelet and kubectl (change X to patch version)
apt-get update && apt-get install -y --allow-change-held-packages kubelet=1.20.x-00 kubectl=1.20.x-00

# Restart the kubelet
sudo systemctl daemon-reload
sudo systemctl restart kubelet

# Verify Controlplane node is upgraded
kubectl get nodes
```
### Upgrading worker nodes
```bash
# Drain node
kubectl drain <node name> --ignore-daemonsets

# SSH into the node
ssh <node name>

# Upgrade kubeadm
apt-get update && apt-get install -y --allow-change-held-packages kubeadm=1.20.x-00

# Upgrade node
sudo kubeadm upgrade node

# Upgrade kubelet and kubectl
apt-get update && apt-get install -y --allow-change-held-packages kubelet=1.20.x-00 kubectl=1.20.x-00

sudo systemctl daemon-reload
sudo systemctl restart kubelet

kubectl uncordon <node-to-drain>
```

### Verify upgrade status
```bash
kubectl get nodes
```
`STATUS` column should show `Ready` for all your nodes and the version number should be updated.

</p>
</details>
