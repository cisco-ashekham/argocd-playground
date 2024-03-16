# README

## Running ArgoCD Autopilot

```ssh
docker run \
  --network="host"
  -e GIT_TOKEN=<TOKEN> \
  -e GIT_REPO=<REPO_HTTP_URL> \
  -v ~/.kube:/home/autopilot/.kube \
  -v ~/.gitconfig:/home/autopilot/.gitconfig \
  -it quay.io/argoprojlabs/argocd-autopilot repo bootstrap
```

## Argo CD LB for external accessibility

Patch the K8s service
```ssh
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

After checking from the AWS console that the LB is created, run the following to get the DNS name
```ssh
kubectl get svc argocd-server -n argocd
```

Finally, run the following to get the password to log in the UI
```ssh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
