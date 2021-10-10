# Application Failures
https://kubernetes.io/docs/tasks/debug-application-cluster/debug-application/

Check Accessibility
```
curl https://mywebapp.com
```

Check Service Accessiblity
```
kubectl get svc web-service -o wide
```

Check the Pod status
```
kubectl get pods -o wide
```
Check the Pod
```
kubectl describe pod web-pod
```
Check logs of Pod
```
kubectl logs web-pod -f --previous
```
Check the manifest of Pod and Services
```
kubectl get pod my-pod -o yaml
```
Fixing issues by editing services or deployments (instead of pods)
```
kubectl -n delta edit deployments.apps webapp-mysql
```

Do the same for the services and pods of Database.

## Examples of Error Message
```
Name does not resolve
```
This means that one of the services or pod names are given wrongly. Check their names and then recreate the pods or services.


```
111 Connection refused
```
This means either the hostname or ports are configured wrongly.
- Check if the targetPort in services are correct.
- Check the selector to see if they are correct.

```
Access denied for user 'sql-user'@'10.244.0.14' (using password: YES)
```
Check if password is correct.

```
# If root user is right:
Access denied for user 'root'@'10.244.0.19' (using password: YES)
```
Recreate the database pod to have the correct password

```
502 Bad Gateway
```
Check if the service's hostname or (node / target) ports are misconfigured.