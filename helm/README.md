# Generic Helm Chart

### Helm supports:

- [x] Kubernetes Volumes as a list 
- [x] Kubernetes Secrets 
- [x] Kubernetes Configmaps 
- [x] Kubernetes Ingress 
- [x] Kubernetes Mount properties
- [x] Kubernetes Init Containers
- [x] Kubernetes Node Selectors 
- [x] Kubernetes Image pull Secret
- [] Helm Dependencies 

### Usage

- Create a Environments-Values/<ENV>-alues.yaml file from Environments-Values/values-dev.yaml
- Install/Upgrade the chart

```
# helm install <Release Name> -f Environments-Values/<ENV>-values.yaml ./
```