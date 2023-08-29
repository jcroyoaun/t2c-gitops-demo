# talks 2 code - Enfoque estrategico a IaC usando GitOps

### Instalando Crossplane

helm repo add crossplane-stable \
    https://charts.crossplane.io/stable

helm repo update

helm upgrade --install \
    crossplane crossplane-stable/crossplane \
    --namespace crossplane-system \
    --create-namespace \
    --wait

kubectl create secret generic digitalocean-creds --from-literal=token=[API-TOKEN] -n crossplane-systems



### Instalando ArgoCD
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


kubectl create secret generic digitalocean-creds --from-literal=token=[YOUR_DIGITALOCEAN_API_TOKEN]

```
export PASS=$(kubectl \
    --namespace argocd \
    get secret argocd-initial-admin-secret \
    --output jsonpath="{.data.password}" \
    | base64 --decode)

```
