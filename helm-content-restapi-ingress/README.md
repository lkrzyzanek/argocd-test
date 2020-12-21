# helm-content-restapi-ingress

Example how to deploy content image + rest api + ingress by using HELM


ArgoCD CLI:

```shell
argocd app create helm-content-restapi-ingress \
  --repo https://github.com/lkrzyzanek/argocd-test.git --path helm-content-restapi-ingress \
  --dest-namespace default --dest-server https://kubernetes.default.svc \
  --helm-set content.image.tag=2.0
```

```shell
argocd app sync helm-content-restapi-ingress
```

## Simulating Pull Request Preview

### New PR Created

Build creates image with version same as PR number and then new app is created:

```shell
argocd app create helm-content-restapi-ingress-pr-30 \
--repo https://github.com/lkrzyzanek/argocd-test.git --path helm-content-restapi-ingress \
--dest-namespace default --dest-server https://kubernetes.default.svc \
--helm-set content.image.pullPolicy=Always --helm-set content.image.tag=3.0 --helm-set ingress.host=pr-30.minikube.info
```

Initial sync:
```shell
argocd app sync helm-content-restapi-ingress-pr-30
```

### New PR Commit

Image is refreshed

```shell
argocd app sync helm-content-restapi-ingress-pr-30
```

### PR is Closed

Image is deleted

```shell
argocd app delete helm-content-restapi-ingress-pr-30
```
