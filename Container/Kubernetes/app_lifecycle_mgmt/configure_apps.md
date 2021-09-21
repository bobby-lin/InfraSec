# Configuring Applications
Remember that by default, containers are alive only when process is alive. The container will exit if the process is exit or completed.

## Configuring Command and Arguments on applications
View the command and arguments of a pod
```
kubectl describe pods xxx
```
### Ovewrite Dockerfile entrypoint and cmd
- The ENTRYPPOINT in Dockerfile can be overwrite by the Pod's `command` parameter.
- The CMD in Dockefile can be overwrite by Pod's `args` parameter.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: ubuntu-pod
spec:
  containers:
    - name: ubuntu
      image: ubuntu
      command: ["sleep"] # corresponds to Docker's ENTRYPOINT["..."]
      args: ["5"] # correspond to Docker's CMD["..."]
```

## Configuring Environment Variables
### Plain Key Value
```
env:
  - name: APP_COLOR
    value: pink
```
### ConfigMap

#### Useful commands
See how many configmaps
```
kubectl get configmaps
```
View specific configmap
```
kubectl describe configmaps db-config
```

Two steps:
1) Create ConfigMap
#### Imperative
From arguments
```
kubectl create configmap <config-name> --from-literal=<key>=<value>
kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MOD=prod
```
From file
```
kubectl create configmap <config-name> --from-file=<path-to-file>
kubectl create configmap app-config --from-file=app_config.properties
```
#### Declarative

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
```

2) Inject into Pods
ENV:
```
envFrom:
  - configMapRef:
      name: app-config
```
Single ENV:
```
env:
  - name: APP_COLOR
    valueFrom: 
        configMapKeyRef:
          name: app-config
          key: APP_COLOR
```
Volume:
```
volumes:
  - name: app-config-volume
    configMap: 
      name: app-config
```
## Configuring Secrets
Note about the risks of K8s secrets: https://kubernetes.io/docs/concepts/configuration/secret/#risks

1) Create Secrets
#### Imperative
From arguments
```
kubectl create secret generic <secret-name> --from-literal=<key>=<value>
kubectl create secret generic app-secret --from-literal=DB_HOST=xxxxx --from-literal=DB_PW=xxxxx
```
From file
```
kubectl create secret generic <secret-name> --from-file=<secret-path>
kubectl create secret generic app-secret --from-file=secrets.properties
```
#### Declarative
Note that the data needs to be base64 encoded.
```
echo -n 'string' | base64
```
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_HOST: xxxxx
  DB_NAME: xxxxx
  DB_PW: xxxxxxxxxxxxxx
```
View Secrets
```
kubectl get secrets
kubectl describe secrets
kubectl get secret app-secret -o yaml
```
```
echo -n 'string' | base64 --decode
```

2) Inject Secrets into Pods
Inject as SINGLE ENV
```yaml
env:
  - name: APP_COLOR
    valueFrom: 
      secretKeyRef:
        name: app-secret
        key: DB_Password
```
Inject as ENV
```yaml
envFrom:
  - secretRef:
      name: app-secret
```
Inject from Volume
> Note: There is a potential problem as each secret will be created as a single file
```yaml
volumes:
- name: app-secret-volume
  secret:
    secretName: app-secret
```

