# tutorial-pipeline

## Azure Cloud Resources

- Resource group : rg-tutorial-cicd-lab
  - Region : Southeast Asia
- Kubernetes service
  - cluster name : aks-tutorial-az-asse-001
  - Location : Southeast Asia
- Container Registry
  - Registry name : acrtutorialazasse001
  - Location : Southeast Asia


## Create Helm Template



helm template . -set ... --output-dir output

```
helm template ./helm/apptemplate --namespace dev --set appname=db --set image.name=mongo --set image.tag=latest --set envVar.name=MONGO_INITDB_ROOT_USERNAME --set envVar.value=root --set envVar.name2=MONGO_INITDB_ROOT_PASSWORD --set envVar.value2=password --set service.port=27017 --set ingress.enabled=false --output-dir helm/output
```