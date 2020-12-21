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
PR_NUMBER=3.0
argocd app create helm-content-restapi-ingress-pr-${PR_NUMBER} \
--repo https://github.com/lkrzyzanek/argocd-test.git --path helm-content-restapi-ingress \
--dest-namespace default --dest-server https://kubernetes.default.svc \
--helm-set deployment.name=helm-content-restapi-ingress-pr-${PR_NUMBER} \
--helm-set content.image.tag=${PR_NUMBER} --helm-set content.image.pullPolicy=Always \
--helm-set ingress.host=pr-${PR_NUMBER}.minikube.info
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
