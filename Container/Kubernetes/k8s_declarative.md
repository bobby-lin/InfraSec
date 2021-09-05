# K8s Declarative objects

Create / Update resources in a file
```
kubectl apply -f file.yaml
```
Create / Update a whole directory of yaml
```
kubectl apply -f file.yaml
```
### Building YAML file
kind: List of resources that the cluster supports
`kubectl api-resources`

apiVersion: API versions that the cluster supports
`kubectl api-versions`

metadata: only name is required

spec: where you specify main actions

### Searching specs in k8s
kubectl explain services

### Dry Run and Diff

Dry-run a create (client side only)
```
kubectl apply -f app.yml --dry-run
```
Dry-run and create a pod.yaml file
```
kubectl run redis --image=redis --dry-run=client -o yaml > pod.yaml
```
Dry-run a create / update on server
```
kubectl apply -f app.yml --server-dry-run
```
Diff to see the differences visually (+-)
```
kubectl diff -f app.yml
```

### Label and Annotations
- Labels are under the `metadata` in your yaml file.
- Simple list of key:value for identifying your resources later when you are selecting, grouping or filtering objects.
- Label Examples:
```
tier:frontend
app:api
env:prod
customer:acme.co
```
- Labels are not meant to hold complex, large or non-identifying info. That is the purpose of **annotations**.

filter a get command
```
kubectl get pods -l app=nginx
```
apply only to matching labels
```
kubectl apply -f myfile.yaml -l app=nginx
```
In yaml file, look for keyword  `matchLabels`.

### Label Selectors
- It tells Services and Deployments which pods belongs to them.
- Many resources use Label Selectors to "link" resource dependencies.
- Use Labels and Selectors to control which pods go to which nodes
- Taints and Tolerations also control node placement
