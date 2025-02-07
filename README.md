# ArgoCD Demo

## Setup

1. Create the namespace:

```bash
kubectl create namespace argocd
```

2. Apply ArgoCD Manifests:

```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

3. Get the initial admin password:
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

4. Access the ArgoCD API Server:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Access `localhost:8080` with username: admin, password: `<see step 3>`.

5. Install the ArgoCD CLI:

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```
