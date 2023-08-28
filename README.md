# talks 2 code - Enfoque estrategico a IaC usando GitOps

### Instalando Crossplane


### Instalando ArgoCD
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


### Create secret
kubectl create secret generic digitalocean-creds --from-literal=token=[YOUR_DIGITALOCEAN_API_TOKEN]
