# argo-cd-sample

## Init

```sh
# Install dependency
helm repo add argo https://argoproj.github.io/argo-helm
kubectl create namespace argocd
helm dependency build charts/argo-cd

# Deploy argo cd chart to cluster
helm template -f charts/argo-cd/values.dev.yaml --namespace=argocd argo-cd charts/argo-cd/
helm install -f charts/argo-cd/values.dev.yaml --namespace=argocd argo-cd charts/argo-cd/

# Deploy root chart to cluster
helm template --values apps/values.dev.yaml apps/ | kubectl apply -f -

# Remove argocd manually deployed
kubectl delete secret --namespace=argocd -l owner=helm,name=argo-cd
```

## Get default password of argocd

```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
