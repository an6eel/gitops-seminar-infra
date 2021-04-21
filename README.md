# gitops-seminar-infra

## Cluster Installation

1. Install kind: https://kind.sigs.k8s.io

2. Create cluster

   `kind create cluster --config=cluster/kind-cluster.yml`
   
3. Add nginx

    `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml`

4. Add Cert manager

    `kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.2/cert-manager.yaml`
    
    `kubectl apply -f cluster/issuer.yml` 
    
5. Install ArgoCD
    
    `kubectl create ns argocd`
    `kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml`
    `kubectl apply -f cluster/ingress.yml`

## Update helm chart 

```
cd charts
helm package api
mv api-VERSION.tgz ../docs
cd ..
helm repo index docs --url https://an6eel.github.io/gitops-seminar-infra/
git commit -am "Update chart"
git push
```