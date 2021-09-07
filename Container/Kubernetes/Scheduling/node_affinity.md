# Node Affinity
https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/

- NodeAffinity Types:
    - requiredDuringSchedulingIgnoredDuringExecution
    - preferredDuringSchedulingIgnoredDuringExecution
    - [Future] requiredDuringSchedulingRequiredDuringExecution
- Suppose the node label does not exist during creation of Pod, then whether the Pod will be created will depend on the NodeAffinity Types.
- Scheduling means when the Pod is first creating
- Execution means when there are updates

# Example: Definition file
Available operators: `In`, `NotIn`, `Exists`, `DoesNotExist`, `Gt`, `Lt`


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: admin-user
spec:
  containers:
  - name: nginx
    image: nginx:latest
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In
            values: 
            - Large
            - Medium # We can add more values here for OR condition
```
Using `NotIn` operator to filter out size=Small labelled nodes.
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: admin-user
spec:
  containers:
  - name: nginx
    image: nginx:latest
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: NotIn
            values: 
            - Small # Any values except for Small
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: blue
  name: blue
spec:
  replicas: 3
  selector:
    matchLabels:
      app: blue
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: blue
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:            
              - matchExpressions:
                 - key: color
                   operator: In
                   values:
                     - blue
```
Create a new deployment on specific label key (notice the value is ignored)
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: red
  name: red
spec:
  replicas: 2
  selector:
    matchLabels:
      app: red
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: red
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:            
              - matchExpressions:
                 - key: node-role.kubernetes.io/master
                   operator: Exists
status: {}
```

## Useful commands
View the labels of a node
```
kubectl describe node node01
```
Label your nodes
```
kubectl label nodes <node-name> <label-key>:<label-value>
kubectl label nodes node01 size=Large
```