# Helm 

## Init Template

```bash
> cd helm

> helm create apptemplate
```

### Get AKS DNS
```bash
az aks show -g rg-tutorial-cicd-lab -n aks-tutorial-az-asse-001 -o tsv --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName
```

## Template Configuraiton via --set

- ``appname``
- ``image.name``=mongo
- ``image.tag``=latest 
- ``envVar.name``=MONGO_INITDB_ROOT_USERNAME 
- ``envVar.value`` 
- ``envVar.name2`` 
- ``envVar.value2`` 
- ``service.type``
- ``service.port`` 
- ``ingress.enabled``



## Run Deploy Helm

### STEP 1 DEPLOY MONGODB

```bash
helm upgrade --install --create-namespace --atomic --wait --namespace tutorial-dev mongodb . --values tutorial/mongodb.yaml --set appname=mongodb --set env=dev --set image.tag=latest --set envVar.value=root --set envVar.value2=password --set buildNumber=1
```
genterate output yaml
```bash
helm template ./helm/apptemplate --values helm/apptemplate/tutorial/mongodb.yaml --namespace tutorial-dev --set appname=mongodb --set env=dev --set image.tag=latest --set envVar.value=root --set envVar.value2=password --set buildNumber=1 --output-dir helm/output
```

deploy with yaml
```bash
kubectl apply -f output/apptemplate/templates/namespace.yaml
kubectl apply -f output/apptemplate/templates/deployment.yaml -n tutorial-dev
kubectl apply -f output/apptemplate/templates/service.yaml -n tutorial-dev
```

### STEP 2 DEPLOY BACKEND

```bash
helm upgrade --install --create-namespace --atomic --wait --namespace tutorial-dev tutorial-backend . --values tutorial/tutorial-backend.yaml --set env=dev --set image.tag=0.0.1-SNAPSHOT --set buildNumber=1
```

genterate output yaml
```bash
helm template ./helm/apptemplate --values helm/apptemplate/tutorial/tutorial-backend.yaml --namespace tutorial-dev --set env=dev --set image.tag=0.0.1-SNAPSHOT --set buildNumber=1 --output-dir helm/output
```
deploy with yaml
```bash
kubectl apply -f output/apptemplate/templates/deployment.yaml -n tutorial-dev
kubectl apply -f output/apptemplate/templates/service.yaml -n tutorial-dev
kubectl apply -f output/apptemplate/templates/ingress.yaml -n tutorial-dev
```

### STEP 3 DEPLOY FRONTEND

```bash
helm upgrade --install --create-namespace --atomic --wait --namespace tutorial-dev tutorial-frontend . --values tutorial/tutorial-frontend.yaml --set env=dev --set image.tag=0.0.1-SNAPSHOT --set buildNumber=1 
```

genterate output yaml
```bash
helm template ./helm/apptemplate --values helm/apptemplate/tutorial/tutorial-frontend.yaml --namespace tutorial-dev --set env=dev --set image.tag=0.0.1-SNAPSHOT --set buildNumber=1 --output-dir helm/output
```
deploy with yaml
```bash
kubectl apply -f output/apptemplate/templates/deployment.yaml -n tutorial-dev
kubectl apply -f output/apptemplate/templates/service.yaml -n tutorial-dev
kubectl apply -f output/apptemplate/templates/ingress.yaml -n tutorial-dev
```
