# DevOps Project | pch-client helm deploy on Kubernetes

```bash
helm dep update .

helm install pchclient . -n pchclient-dev --create-namespace
helm upgrade pchclient . -n pchclient-dev
```

## Future improvements

- Fix issue on hasure when using postgresql-ha (helm)
- Use helm secrets

## Secrets management

It is based on **helm-secrets** plugin [https://github.com/jkroepke/helm-secrets](https://github.com/jkroepke/helm-secrets).

Also check [this tutorial](https://developer.epages.com/blog/tech-stories/kubernetes-deployments-with-helm-secrets/).
