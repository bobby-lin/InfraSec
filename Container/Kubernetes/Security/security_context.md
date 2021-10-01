# Security context
Security context at Pod level will overwrite the Security context at container level.

Configure at Pod level
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: k8s-dashboard
spec:
  securityContext:
    runAsUser: 1000
  containers:
    - name: k8s-dashboard
      image: k8s-dashboard
```
Configure at Container level
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: k8s-dashboard
spec:
  containers:
    - name: k8s-dashboard
      image: k8s-dashboard
    - securityContext:
        runAsUser: 1000
        capabilities:
          add: ["MAC_ADMIN"] # only supported at container level
```

Find the default user in the container
```
kubectl exec ubuntu-sleeper -- whoami
```

Root user is `0`.