# setup postgresql

Add helm repos:
```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

Install mysql and postgresql:
```bash
helm install postgresql bitnami/postgresql \
  --set postgresqlPassword=postgres \
  --set postgresqlDatabase=magma
```
