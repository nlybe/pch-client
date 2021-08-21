# Postgresql HA

Based on [https://github.com/bitnami/charts/tree/master/bitnami/postgresql-ha](https://github.com/bitnami/charts/tree/master/bitnami/postgresql-ha)

## Deployment commands

```bash
# Create the PersistentVolume
kubectl apply -f pv-postgresqlha.yaml

helm repo add bitnami https://charts.bitnami.com/bitnami

helm install postgresqlha bitnami/postgresql-ha -n db --create-namespace -f values.yaml
helm upgrade postgresqlha bitnami/postgresql-ha -n db -f values.yaml
```

## Notes after helm creation

```bash
** Please be patient while the chart is being deployed **
PostgreSQL can be accessed through Pgpool via port 5432 on the following DNS name from within your cluster:

    postgresqlha-postgresql-ha-pgpool.db.svc.cluster.local

Pgpool acts as a load balancer for PostgreSQL and forward read/write connections to the primary node while read-only connections are forwarded to standby nodes.

To get the password for "postgres" run:

    export POSTGRES_PASSWORD=$(kubectl get secret --namespace db postgresqlha-postgresql-ha-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode)

To get the password for "repmgr" run:

    export REPMGR_PASSWORD=$(kubectl get secret --namespace db postgresqlha-postgresql-ha-postgresql -o jsonpath="{.data.repmgr-password}" | base64 --decode)

To connect to your database run the following command:

    kubectl run postgresqlha-postgresql-ha-client --rm --tty -i --restart='Never' --namespace db --image docker.io/bitnami/postgresql-repmgr:11.13.0-debian-10-r0 --env="PGPASSWORD=$POSTGRES_PASSWORD"  \
        --command -- psql -h postgresqlha-postgresql-ha-pgpool -p 5432 -U postgres -d postgres

To connect to your database from outside the cluster execute the following commands:

    kubectl port-forward --namespace db svc/postgresqlha-postgresql-ha-pgpool 5432:5432 &
    psql -h 127.0.0.1 -p 5432 -U postgres -d postgres
```
