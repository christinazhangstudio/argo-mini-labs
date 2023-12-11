## start

created a local K8s cluster using minikube

```
Creating hyperv VM (CPUs=2, Memory=2200MB, Disk=20000MB)
Preparing Kubernetes v1.28.3 on Docker 24.0.7
...
* Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

```
kubectl create namespace argocd
```

install argocd (https://github.com/argoproj/argo-cd/releases):
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

verify pods exist:
```
kubectl get pods -n argocd
```

port-forward argocd:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

argocd comes with a built in admin user and pass:
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"
echo (paste-base64-val-here) | openssl base64 -d
set argocd_password=(that-decoded-val)
```

login:
```
argocd login  --insecure --username=admin --password=%argocd_password% localhost:8080
'admin:login' logged in successfully
Context 'localhost:8080' updated
```

can verified login'd by:
```
argocd cluster list
```

the cluster is running on address (aka internal k8s api endpoint) is `https://kubernetes.default.svc`

also can see ArgoCD Web UI using admin/%argocd_password% at http:localhost:8080

so now can view a cluster with no apps!

## adding an app




