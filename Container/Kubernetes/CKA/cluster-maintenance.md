# Exercises

## Upgrading Kubernetes cluster with Kubeadm

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

## OS Upgrade

<details><summary>Solution</summary>
<p>

### Make a Node to become unschedulable
```bash
# Also move pods to other clusters
kubectl drain <node name> --ignore-daemonset

# Do not move the pods
kubectl cordon <node name>
```

### Upgrade the OS of node
```
# Stop Kubernetes from being updated
sudo apt-mark hold kubeadm kubelet kubectl 
sudo apt update && sudo apt upgrade -y

# Reboot if required
sudo reboot
```

### Set Worker's node to be schedulable again
```
kubectl uncordon <node name>
```

</p>
</details>
