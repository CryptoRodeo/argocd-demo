# ArgoCD Demo

## Setup
### Minikube


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

5. Get the service URL
```bash
# For Minikube
# Get the service URL
 minikube service argocd-server-nodeport -n argocd
```

6. Install the ArgoCD CLI:

```bash
curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
rm argocd-linux-amd64
```

7. Access `<ip>:30080` with username: admin, password: `<see step 3>`:
```bash
argocd login <service-ip>:30080 --username admin --password <password> --insecure
```

8. Add your cluster to ArgoCD:
```bash
argocd cluster add <cluster-name, minikube for example>
```

9. Get your cluster's server URL and port:
```bash
kubectl config view -o jsonpath='{.clusters[?(@.name=="minikube")].cluster.server}'
```

10. Add your application:

```bash
argocd app create nginx-demo \                                                                     
--repo https://github.com/cryptorodeo/argocd-demo \
--path . \
--dest-server https://<cluster-url> \
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
