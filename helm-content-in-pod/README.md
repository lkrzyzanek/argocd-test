# helm-content-in-pod

Example how to deploy content image initiating data into pod with no Persistent Volume using HELM


ArgoCD CLI:

```shell
argocd app create helm-content-in-pod \
  --repo https://github.com/lkrzyzanek/argocd-test.git --path helm-content-in-pod \
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
PR_NUMBER=54378
CONTENT_IMAGE_TAG=3.0
argocd app create helm-content-restapi-pr-${PR_NUMBER} \
--repo https://github.com/lkrzyzanek/argocd-test.git --path helm-content-in-pod \
--dest-namespace default --dest-server https://kubernetes.default.svc \
--helm-set deployment.name=content-restapi-pr-${PR_NUMBER} \
--helm-set content.image.tag=${CONTENT_IMAGE_TAG} --helm-set content.image.pullPolicy=Always \
--helm-set ingress.host=pr-${PR_NUMBER}.minikube.info
```

Initial sync:
```shell
argocd app sync helm-content-restapi-pr-${PR_NUMBER}
```

### New PR Commit

Image is refreshed

```shell
argocd app sync helm-content-restapi-pr-${PR_NUMBER}
kubectl rollout restart deployment helm-content-restapi-pr--${PR_NUMBER}-http
```

### PR is Closed

Image is deleted

```shell
argocd app delete helm-content-restapi-pr-${PR_NUMBER}
```
