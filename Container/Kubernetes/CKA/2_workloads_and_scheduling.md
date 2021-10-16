# Workloads & Scheduling (15%)

# Understand deployments and how to perform rolling update and rollbacks
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

## Use ConfigMaps and Secrets to configure applications

## Know how to scale applications

## Understand the primitives used to create robust, self-healing, application deployments

## Understand how resource limits can affect Pod scheduling

## Awareness of manifest management and common templating tools
