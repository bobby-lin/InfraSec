# Services - Nodeport
https://kubernetes.io/docs/concepts/services-networking/service/

Service is an abstract way of exposing a set of Pods to network (outside of the cluster).

- Nodeport default port range: 30000 - 32767
- There is a specified targetPort on the pods.

## Example of service definition
Suppose you have a Pod:
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
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  ports:
    - protocol: TCP
      port: 80 # Only port is mandatory field
      nodePort: 30008
      targetPort: 9999
  selector:
    app: myapp
    type: front-end
```

## Useful Commands
Finding info about a service such as the `TargetPort`
```
kubectl describe services kubernetes
```