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

4. Access the ArgoCD API Server using a NodePort service:
```
kubectl apply -f NodePort.yml
```

5. Install the ArgoCD CLI:

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

6. Access `<ip>:30080` with username: admin, password: `<see step 3>`:
```bash
argocd login <ip>:30080 --username admin --password <password> --insecure
```

7. Add your cluster to ArgoCD:
```bash
argo cluster add <cluster-name, minikube for example>
```

8. Get your cluster's server URL:
```bash
kubectl config view -o jsonpath='{.clusters[?(@.name=="<cluster-name>")].cluster.server}'
# Example:
kubectl config view -o jsonpath='{.clusters[?(@.name=="minikube")].cluster.server}'
```

9. Add your application:

```bash
argocd app create nginx-demo \                                                                     
--repo https://github.com/cryptorodeo/argocd-demo \
--path . \
--dest-server https://<cluster-url> \
--dest-namespace argocd
```

10. Review your ArgoCD instance and confirm that your application is live.
