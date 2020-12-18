# argocd-test

Testing [Argo CD](https://argoproj.github.io/argo-cd/getting_started/).


## Minikube

1. Start minikube
```shell
minikube config set driver hyperkit
minikube start --addons ingress,dashboard
```

2. Tunnel minikube
```shell
minikube tunnel
```

3. Install [argoCD](https://argoproj.github.io/argo-cd/getting_started/)

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

get pod name (initial password)
```shell
kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2
```

4. Start k8s dashboard

```shell
minikube dashboard
```

Navigate to Services / argocd-server and click on External endpoint to access Argo CD
