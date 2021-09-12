# K8s Resource limits
https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/
kube
> "When you specify the resource request for Containers in a Pod, the scheduler uses this information to decide which node to place the Pod on. When you specify a resource limit for a Container, the kubelet enforces those limits so that the running container is not allowed to use more of that resource than the limit you set. The kubelet also reserves at least the request amount of that system resource specifically for that container to use."

Resources in each node:
- CPU (0.5)
- Memory (256Mi)
- Disk storage

## Resource limits
We can specify default limits in namespaces:
CPU -> Default limit is 1 vCPU (https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/cpu-default-namespace/)
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
```

Memory -> Default limit is 512 Mi (https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/)
```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```

## Example
In the pod definition, add the `resources` field under `spec`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
  labels:
    app: myapp
    type: front-end
spec:
  containers:
    - name: nginx-container
      image: nginx
      resources:
        requests: # this is used by scheduler to decide which node to use
          memory: "1Gi"
          cpu: 1
        limits: # this is the limit that the pod can consume. 
          memory: "128Mi" # If the consumption exceeds memory limit, then the pod is terminated
          cpu: "500m"
```