# talks 2 code - Enfoque estrategico a IaC usando GitOps

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
kubectl create namespace argocd; kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
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
