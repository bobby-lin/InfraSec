# Service - Loadbalancer
This is only supported on Cloud platforms such as AWS, GCP and Azure.

In unsupported environment, this will be treated like another Nodeport.

## Example of Loadbalancer definition file
```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30008
```