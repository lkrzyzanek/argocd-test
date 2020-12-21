# argocd-test

Testing [Argo CD](https://argoproj.github.io/argo-cd/getting_started/).

|Use Case|Description|
|--------|-----------|
|[content](content)|Static content CD|
|[content-restapi-ingress](content-restapi-ingress)|Static content + REST API + Ingress CD|
|[helm-content-restapi-ingress](helm-content-restapi-ingress)|Helm example CD|

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

Create `NEW APP` with:
1. git repo - `https://github.com/lkrzyzanek/argocd-test.git`
2. path - choose the example e.g. `content`

5. Update /etc/host (only if testing ingress)

```shell
echo "$(minikube ip) minikube.info pr-54378.minikube.info" | sudo tee -a /etc/hosts
```
