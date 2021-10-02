# Network Policy
https://kubernetes.io/docs/concepts/services-networking/network-policies/

## Ingress and Egress traffic
Traffic Example:

Client -> (Ingress for Web server) Web server (Egress for Web server) -> (Ingress for API) API server (Egress for API) -> (Ingress for DB) Database Server

Summary of requirements:
- Ingress rule on port 80 at web server
- Egress rule on port 5000 at web server to API server
- Ingress rule on port 5000 at API server
- Egress rule on port 3306 at API server to DB server
- Ingress rule on port 3306 at DB server

## Kubernetes Network
By default, all pods can communicate with each other.

We need to use `network policy` to control the traffic.
Example: only allow ingress traffic from API Pod on port 3306.

## Network Policy
When creating the policy, we only need to think about the direction of the request. The response is automatically assumed as granted.


```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    # Both pod and namespace rules must be matched
    - podselector:
        matchLabels:
          name: api-pod
      namespaceselector:
        matchLabels:
          name: prod
    # Only one ipBlock rule
    - ipBlock:
        cidr: 192.168.5.10/32
    ports:
      - protocol: TCP
        port: 3306
    # Connection to backup server at port 80
    egress:
    - to:
      - ipBlock:
          cidr: 192.168.5.10/32
      ports:
        - protocol: TCP
          port: 80
```
Example of allowing internal service to access payroll at port 8080 and database at port 3306 
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  egress:
  - ports:
    - port: 8080
      protocol: TCP
    to:
    - podSelector:
        matchLabels:
          name: payroll
  - ports:
    - port: 3306
      protocol: TCP
    to:
    - podSelector:
        matchLabels:
          name: mysql
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
```

Add `ingress: from:` or `egress: to:` to network def file.

Four kinds of selectors:
- podSelector
- namespaceSelector
- namespaceSelector and podSelector
```yaml
 ...
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          user: alice
      podSelector:
        matchLabels:
          role: client
  ...
```
Separate policy:
```yaml
  ...
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          user: alice
    - podSelector:
        matchLabels:
          role: client
  ...
```
- ipBlock (using cluster-external IP)

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```

## Useful commands
Get hostname of pod (look at IP)
```
kubectl get pods -o wide  
```

## Network solution that are supported in K8s
- kube-router
- Calico
- Romana
- Weave-net

Not supported:
- flannel