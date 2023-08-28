# talks 2 code - Enfoque estrategico a IaC usando GitOps

### Instalando Crossplane


### Instalando ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


### Crear kubernetes secret para permitir a Crossplane comunicarse con Digital Ocean 
kubectl create secret generic digitalocean-creds --from-literal=token=[YOUR_DIGITALOCEAN_API_TOKEN]
