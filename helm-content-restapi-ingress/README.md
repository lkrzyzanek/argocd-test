# helm-content-restapi-ingress

Example how to deploy content image + rest api + ingress by using HELM


ArgoCD CLI:

```shell
argocd app create helm-guestbook \
  --repo https://github.com/lkrzyzanek/argocd-test.git --path helm-content-restapi-ingress \
  --dest-namespace default --dest-server https://kubernetes.default.svc \
  --helm-set restapi.replicas=2
```