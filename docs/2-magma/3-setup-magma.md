# setup magma

setup volumes:
```bash
kubectl apply -f volume_claims.yaml
```

add orc8r repo:
```bash
helm repo add orc8r https://artifactory.magmacore.org/artifactory/helm/
helm repo update
```

install orc8r:
```bash
helm upgrade -i orc8r orc8r/orc8r -f values.yaml \
  --version=1.5.23 \
  --set nginx.spec.hostname=controller.magmalocal.com
```

create user:
```bash
ORC_POD=$(kubectl get pod -l app.kubernetes.io/component=orchestrator -o jsonpath='{.items[0].metadata.name}')
kubectl exec -it ${ORC_POD} -- envdir /var/opt/magma/envdir /var/opt/magma/bin/accessc \
  add-existing -admin -cert /var/opt/magma/certs/admin_operator.pem admin_operator

NMS_POD=$(kubectl get pod -l app.kubernetes.io/component=magmalte -o jsonpath='{.items[0].metadata.name}')
kubectl exec -it ${NMS_POD} -- yarn setAdminPassword master admin admin
kubectl exec -it ${NMS_POD} -- yarn setAdminPassword magma-test admin admin
```
