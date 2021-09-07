# Node Selector
Sometimes you want to allocate a Pod to a specific node.

But this method has limitations such as you cannot perform `OR` (e.g. size=Large or size=Medium) and `NOT` (e.g. size is not Small) selection. Instead we will use Node Affinity

# Add Node Selector in Pod
Use the `nodeSelector` field under `spec`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: admin-user
spec:
  containers:
  - name: nginx
    image: nginx:latest
  nodeSelector:
    size: Large
```

## Useful commands
Label your nodes
```
kubectl label nodes <node-name> <label-key>:<label-value>
kubectl label nodes node01 size=Large
```