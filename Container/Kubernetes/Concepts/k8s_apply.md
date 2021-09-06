# K8s Apply
https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/

## How does it work?
These configurations are required in order for Kubernetes to decide what changes are required:
- Live object configuration (YAML format in Kubernetes)
    - Compare to Local file and decide what to modify
- Last applied configuration (stored as JSON in the Live object configuration)
    - Helps us to figure out what are the fields that are removed
- Local file (your YAML file)