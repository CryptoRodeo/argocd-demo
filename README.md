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
kubectl apply -f service.yml
```

5. Get the IP for your cluster
```bash
# For Minikube
minikube ip

# For Microk8s
hostname -I | awk '{print $1}'
```

6. Install the ArgoCD CLI:

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

7. Access `<ip>:30080` with username: admin, password: `<see step 3>`:
```bash
argocd login <ip>:30080 --username admin --password <password> --insecure
```

8. Add your cluster to ArgoCD:
```bash
argocd cluster add <cluster-name, minikube for example>
```

9. Get your cluster's server URL:
```bash
kubectl config view -o jsonpath='{.clusters[?(@.name=="<cluster-name>")].cluster.server}'

# Minikube:
kubectl config view -o jsonpath='{.clusters[?(@.name=="minikube")].cluster.server}'

# Microk8s
kubectl config view -o jsonpath='{.clusters[?(@.name=="microk8s-cluster")].cluster.server}'
```

10. Add your application:

```bash
argocd app create nginx-demo \                                                                     
--repo https://github.com/cryptorodeo/argocd-demo \
--path . \
--dest-server https://<cluster-url> \
--dest-namespace argocd
```

11. Confirm that your application is live on ArgoCD
```bash
argocd app list
```

12. Run a sync
```bash
argocd app sync nginx-demo
```

13. Monitor the deployment
```bash
argocd app get nginx-demo
kubectl get po -n argocd
```
