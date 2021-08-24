# DevOps Project | pch-client helm deploy on Kubernetes

```bash
helm dep update .

helm install pchclient . -n pchclient-dev --create-namespace
helm upgrade pchclient . -n pchclient-dev
```

Check pods status

```bash
kubectl get pods -n pchclient-dev
```

In order to check the UIs, add the following entry to local /etc/hosts file, required for local K8s registry and URLS used on ingress configuration

```bash
127.0.0.1       hasura.local
127.0.0.1       pchclientui.local
```

And then use the  URLs below:

- [http://hasura.local:8080](http://hasura.local:8080)
- [http://pchclientui.local:8080](http://pchclientui.local:8080)

## Future improvements

- Fix db permissions issue on hasura when using postgresql-ha (helm)
- Use helm secrets
