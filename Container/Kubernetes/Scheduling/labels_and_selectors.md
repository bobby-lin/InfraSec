# Labels and Selectors
- ReplicaSet uses label and selectors to know what pods to create and maintain. This can be found under the `spec -> template -> labels` and `spec -> selector -> matchLabels` field
- Services also uses selector to identify the ReplicaSet.

> Note: If you want to record some information in the definition file, please use Annotations.

## Example of pod def file with labels
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: "2021-09-07T01:12:56Z"
  labels:
    run: mypod
    mylabel: demo123 # We added this label
  name: mypod
  namespace: default
  resourceVersion: "52732"
  uid: 4d03857b-c567-4232-b61a-f1dff2b431e6
spec:
  containers:
  - image: nginx
    imagePullPolicy: Always
    name: mypod
    resources: {}
    terminationMessagePath: /dev/termination-log
    terminationMessagePolicy: File
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: kube-api-access-ghpps
      readOnly: true
  dnsPolicy: ClusterFirst
  enableServiceLinks: true
  preemptionPolicy: PreemptLowerPriority
  priority: 0
  restartPolicy: Always
  schedulerName: default-scheduler
  securityContext: {}
  serviceAccount: default
  serviceAccountName: default
  terminationGracePeriodSeconds: 30
  tolerations:
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
  - effect: NoExecute
    key: node.kubernetes.io/unreachable
    operator: Exists
    tolerationSeconds: 300
  volumes:
  - name: kube-api-access-ghpps
    projected:
      defaultMode: 420
      sources:
      - serviceAccountToken:
          expirationSeconds: 3607
          path: token
      - configMap:
          items:
          - key: ca.crt
            path: ca.crt
          name: kube-root-ca.crt
      - downwardAPI:
          items:
          - fieldRef:
              apiVersion: v1
              fieldPath: metadata.namespace
            path: namespace
status:
  phase: Pending
  qosClass: BestEffort
```

## Useful commands
Get the pods that has a specific label using selector
```
kubectl get pods --selector mylabel=demo123
```
Get all objects that contain a label
```
kubectl get all --selector env=prod
```
Get pods with multiple labels (use commas)
```
kubectl get pods --selector env=prod,bu=finance,tier=frontend
```

## Common error
This means the selector's matchLabels and template's labels are not matching.
```
The ReplicaSet "replicaset-1" is invalid: spec.template.metadata.labels: Invalid value: map[string]string{"tier":"nginx"}: `selector` does not match template `labels`
```