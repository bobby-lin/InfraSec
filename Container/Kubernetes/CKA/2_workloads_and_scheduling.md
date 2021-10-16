# Workloads & Scheduling (15%)

# Deployments
Doc: https://kubernetes.io/docs/reference/kubectl/cheatsheet/

### Create a deployment & upgrade the image version with annotation
<details><summary>Solution</summary>
<p>

```bash
# Create deployment
kubectl run deployment [name of deployment] [image]:[version]

# Update image with annotation
kubectl set image deployments/[name of deployment] [container name]=[image][new version] --record
```

</p>
</details>
