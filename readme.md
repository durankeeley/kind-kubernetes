# Install Kind (Kubernetes in Docker)
## Prereq: 
- Go
- Docker

[Kind Homepage](https://kind.sigs.k8s.io/)

# Creation of cluster for Argo CD
```
kind create cluster `
  --name argocd `
  --config kind-cluster.yaml
```

## Confirm working of Kind Cluster
```
kubectl cluster-info `
  --context kind-argocd
```

# Install Argo CD
## Creation of kube namespace
```
kubectl create namespace argocd
```
## Follow getting started guide
[Argo CD Getting Started](https://argo-cd.readthedocs.io/en/stable/getting_started/)
```
kubectl apply `
  -n argocd `
  -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## PowerShell patch the service to use localhost:8080
```
kubectl patch svc argocd-server -n argocd -p `
  '{\"spec\": {\"type\": \"NodePort\", \"ports\": [{\"name\": \"https\", \"nodePort\": 30443, \"port\": 443, \"protocol\": \"TCP\", \"targetPort\": 8080}]}}'

```

## Getting the Argo CD Password with PowerShell
```
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String((kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"))) | Write-Output
```

# Install Application in Argo CD
## Creation of application namespace
```
kubectl create namespace application1
```

Git repo
```
https://github.com/.git
```