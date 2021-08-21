# DevOps Project | pch-client on Kubernetes

For the following steps is supposed the data is located on "/home/nikos/data/projects/devops/codeBGP/data" folder.
If this path is different, the it should be replaced accordingly on **clusterK3d/codebgp-k3d.yaml** file.

## Deploy a K8s cluster locally and pch-client apps

Used the tool **k3d** using the following command:

```bash

# 1. Add the following entry to local /etc/hosts file, required for local K8s registry and URLS used on ingress configuration
127.0.0.1       k3d-codebgp-registry # if use k3d local registry
127.0.0.1       hasura.local
127.0.0.1       pchclientui.local

# 2. Create the cluster using the following:
k3d cluster create --config ./clusterK3d/codebgp-k3d.yaml

# 3. Create the namespace and set it as default
kubectl create ns pchclient-dev
kubens pchclient-dev

# 4. If the "pch-client_parser" and "pch-client_ui" images exists locally do the following:
# 4.1 Check the k3d command output or docker ps -f name=k3d-mycluster-registry to find the exposed port (e.g. 44257) 
# 4.2 Update registry local images using the exposed port as follows:
docker tag pch-client_parser k3d-codebgp-registry:44257/pchclientparser:local
docker push k3d-codebgp-registry:44257/pchclientparser:local
docker tag pch-client_ui k3d-codebgp-registry:44257/pchclientui:local
docker push k3d-codebgp-registry:44257/pchclientui:local

# 5. On Deployment commands
kubectl apply -f config/pv-postgresql-db.yaml
kubectl apply -f config/dev-cm-graphql.yaml
kubectl apply -f config/dev-cm-pchclient-parser.yaml
kubectl apply -f config/dev-cm-postgresql-db.yaml
kubectl apply -f config/dev-configmap.yaml
kubectl apply -f config/dev-secrets.yaml
kubectl apply -f config/ss-postgresql-db.yaml
kubectl apply -f config/srv-graphql.yaml
kubectl apply -f config/srv-postgresql-db.yaml
kubectl apply -f config/srv-pchclient-ui.yaml
kubectl apply -f config/dpl-graphql.yaml
kubectl apply -f config/dpl-pchclient-parser.yaml
kubectl apply -f config/dpl-pchclient-ui.yaml
kubectl apply -f config/dev-ingress.yaml
```

## Check the status of deployment

### Check pods status

```bash
kubectl get pods
```

### Open the following URLs

- [http://hasura.local:8080](http://hasura.local:8080)
- [http://pchclientui.local:8080](http://pchclientui.local:8080)

## Various helper commands

```bash
# Pull image inside a node
crictl pull nginx

# Various commands
kubectl port-forward svc/graphql 8081:8080 -n pchclient-dev
kubectl port-forward svc/pchclient-ui 8083:80 -n pchclient-dev

kubectl port-forward svc/kubernetes-dashboard 8000:8000 -n kubernetes-dashboard
kubectl port-forward svc/kube-prometheus-stack-grafana 8001:80 -n default

# Install prometheus stack
helm install kube-prometheus-stack prometheus-community/kube-prometheus-stack

# Get grafana default password
kubectl get secret --namespace default kube-prometheus-stack-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo


```