# github-actions-headlamp

kubectl create secret generic headlamp-token \
  --type kubernetes.io/service-account-token \
  --namespace headlamp \
  --from-literal=usage=login \
  --dry-run=client -o yaml |
  tee headlamp-token.yaml

nano headlamp-token.yaml

Y agrega esto dentro de metadata:

  annotations:
    kubernetes.io/service-account.name: headlamp

kubectl apply -f headlamp-token.yaml

kubectl get secret headlamp-token -n headlamp -o jsonpath='{.data.token}' | base64 --decode
