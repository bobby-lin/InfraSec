# ClusterIP
The ClusterIP service is created by the cluster as default.

## Example of ClusterIP definition file
```yaml
apiVersion: v1
kind: Service
metadata:
  name: back-end
spec:
  type: ClusterIP
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80 # The exposed backend port
  selector:
    app: myapp
    type: back-end
```