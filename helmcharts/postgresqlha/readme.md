# Postgresql HA

Based on [https://github.com/bitnami/charts/tree/master/bitnami/postgresql-ha](https://github.com/bitnami/charts/tree/master/bitnami/postgresql-ha)

## Deployment commands

```bash
# Create the PersistentVolume
kubectl apply -f pv-postgresqlha.yaml

# Deploy using helm
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install postgresqlha bitnami/postgresql-ha -n db --create-namespace -f values.yaml

# Upgrade command
helm upgrade postgresqlha bitnami/postgresql-ha -n db -f values.yaml
```
