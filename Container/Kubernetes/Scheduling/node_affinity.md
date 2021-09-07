# Node Affinity
https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/

- NodeAffinity Types:
    - requiredDuringSchedulingIgnoredDuringExecution
    - preferredDuringSchedulingIgnoredDuringExecution
    - [Future] requiredDuringSchedulingRequiredDuringExecution
- Suppose the node label does not exist during creation of Pod, then whether the Pod will be created will depend on the NodeAffinity Types.
- Scheduling means when the Pod is first creating
- Execution means when there are updates

## Taints / Tolerations + Node Affinity
Suppose we have a scenario where we want pods (with toleration) to be scheduled in specific nodes (that are tainted).

But there are also pod that does not have toleration and nodes which are untainted.

1) Using Taints / Toleration only
- Taints: set the node with taint to restrict which pods can be accepted
- Tolerations: set the pod with respective toleration

Node will accept the pods with the matching tolerations.

> Problem: However there is a chance that one of the pods (with toleration) will end up in an untainted node.

2) Using Node Affinity only
- Label: Add label to your node
- Affinity: Add nodeAffinity to the `spec` field of the Pod def. file.

Pod will be created based on the specified operators in the Nodes.

> Problem: It is possible that other pods (without NodeAffinity) will be created in the labelled node (something you don't want)

### Solution
We can use Taints / Tolerations with Node Affinity.
- Taints / Tolerations ensure that the Node will accept certain tolerated Pods.
    - The problem that NodeAffinity had will be fixed.
- NodeAffinity will ensure that the Pods will be created in specific labelled Node.
    - This solve the problem that Taints / Tolerations have.

## Example: Definition file
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