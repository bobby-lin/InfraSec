# K8s Deployment
Purpose of Deployment:
- Deploying new servers
- Rolling updates
- Rollback changes
- Resume changes

## Example of deployment definition file
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: nginx
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 9999
```

## Commands
Create deployment with definition file
```
kubectl create -f deployment-definition.yml
```
View the deployment
```
kubectl get deployments
```
We can also see that the replicaset is created
```
kubectl get replicaset
```
The underlying pods are also created
```
kubectl get pods
```
See all created objects
```
kubectl get all
```
Create a YAML file for deployment with specified replicaset and image
```
kubectl create deployment --image=httpd:2.4-alpine httpd-frontend --replicas=3 --dry-run=client -o yaml > httpd-frontend.yaml
```

## Errors
Check the typo in the `kind` parameter - the "deployment" should be capitalized.
```
Error from server (BadRequest): error when creating "deployment-definition-1.yaml": deployment in version "v1" cannot be handled as a Deployment: no kind "deployment" is registered for version "apps/v1" in scheme "k8s.io/kubernetes/pkg/api/legacyscheme/scheme.go:30"
```