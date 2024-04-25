# DevOps task

## Prerequisites
- K8S cluster
- Helm command line tool
- Docker

## Build Docker image

1. Build countries and airports images
```
docker build -f countries/Dockerfile -t quay.io/dyaaahmed/countries:1.0.1
docker build -f airports/airports-1.0.1/Dockerfile -t quay.io/dyaaahmed/airports:1.0.1
docker build -f airports/airports-1.1.0/Dockerfile -t quay.io/dyaaahmed/airports:1.1.0
```
2. Push the images

```
docker push quay.io/dyaaahmed/countries:1.0.1
docker push quay.io/dyaaahmed/airports:1.0.1 
```

## Deploy to K8S:

1. Create 2 namespaces
```
kubectl apply -f K8S/countries-ns.yaml -f K8S/airports-ns.yaml
```

2. Deploy the services with helm
```
kubectl label node <node_name> env=demo
helm install countries -f countries/values.yaml -n countries-ns
helm install airports -f airports/values.yaml -n airports-ns
```

3. Deploy Nginx ingress controller 
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.0/deploy/static/provider/baremetal/deploy.yaml
``` 
For minikube, `minikube addons enable ingress`
 
4. Apply network policies
```
kubectl label ns ingress-nginx name=ingress-nginx
kubectl apply -f K8S/countries-networkpolicy.yaml -n countries-ns
kubectl apply -f K8S/airports-networkpolicy.yaml -n airports-ns
```

5. Update the airports image
```
helm upgrade airports --set image.tag=1.1.0 ./helm
``` 

## CICD with GitlabCI
1. Add the value of QUAY_USERNAME and QUAY_PASSWORD to gitlab environment secrets
2. Check .gitlab-ci.yaml
