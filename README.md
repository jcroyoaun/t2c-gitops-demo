# talks 2 code - Enfoque estrategico a IaC usando GitOps
Este es un demo sencillo que pretende ilustrar una estrategia de usar Crossplane para poder implementar las practicas GitOps en ArgoCD, permitiendo manejar recursos externos a objetos de kubernetes, en este caso especifico, un Droplet (Linux VM) de Digitalocean.

### Pre requisitos
Durante el demo, usaremos un entorno local para comunicarnos con infraestructura remota. Es necesario contar con:

* Algun Kubernetes local como kind (yo estoy usando Rancher Desktop)
* Helm
* Una cuenta de DigitalOcean


### Instalando Crossplane

```bash
helm repo add crossplane-stable https://charts.crossplane.io/stable
```

```bash
helm repo update
```

```bash
helm upgrade --install \
    crossplane crossplane-stable/crossplane \
    --namespace crossplane-system \
    --create-namespace \
    --wait
```

Create secret with API token
```bash
kubectl create secret generic digitalocean-creds --from-literal=access-token=[API-TOKEN] -n crossplane-systems
```


### Instalando ArgoCD
```bash
kubectl kustomize argo-deployment/ | kubectl apply -f -
```

En el repo, cuento con un app.yaml y un project.yaml custom, donde le indico a Argo en que repositorio observar mis cambios. Despues de inspeccionar estos archivos yaml, hagamos kubectl apply para crear la app y el proyecto.
```bash
kubectl apply -f app.yaml
kubectl apply -f project.yaml
```

Obtenemos el password temporal de argo para poder accederlo desde La Linea De Comandos.
```
export PASS=$(kubectl \
    --namespace argocd \
    get secret argocd-initial-admin-secret \
    --output jsonpath="{.data.password}" \
    | base64 --decode)
```



## Creando Aplicacion y Proyecto de ArgoCD
```
kubectl apply -f app.yaml
kubectl apply -f project.yaml
```


## Digital Ocean API slugs
https://slugs.do-api.dev/


## APPENDIX

#### Para borrar argocd

```
kubectl delete namespace argocd
```
Es posible que el namespace se quede en "terminating", en dado caso, se puede aplicar este finalizador directamente en la API de argocd
```
kubectl get namespace argocd -o json | jq 'del(.spec.finalizers[])' | curl -k -H "Content-Type: application/json" -X PUT --data-binary @- http://127.0.0.1:8001/api/v1/namespaces/argocd/finalize
```


#### para borrar los crds de argo
```
kubectl delete crd applications.argoproj.io applicationsets.argoproj.io appprojects.argoproj.io
```

en caso de que se queden atorados, usar
```
kubectl get crd appprojects.argoproj.io -o json | jq '.metadata.finalizers=null' | kubectl apply -f -
```

y

```
kubectl get crd applications.argoproj.io -o json | jq '.metadata.finalizers=null' | kubectl apply -f -
```

respectivamente.
