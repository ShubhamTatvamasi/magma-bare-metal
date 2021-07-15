### setup secrets

generate secrets:
```bash
mkdir -p certs && cd certs
../1-generate-secrets.sh magmalocal.com
```

add orc8r secrets repo:
```bash
helm repo add magma-charts-152 https://shubhamtatvamasi.github.io/magma-charts-152
helm repo update
```

create new namespace:
```bash
kubectl create ns orc8r
kubens orc8r
```

create orc8r secret:
```bash
helm template orc8r magma-charts-152/secrets \
  --set secret.certs.enabled=true \
  --set-file secret.certs.files."rootCA\.pem"=rootCA.pem \
  --set-file secret.certs.files."rootCA\.key"=rootCA.key \
  --set-file secret.certs.files."controller\.crt"=controller.crt \
  --set-file secret.certs.files."controller\.key"=controller.key \
  --set-file secret.certs.files."admin_operator\.pem"=admin_operator.pem \
  --set-file secret.certs.files."admin_operator\.key\.pem"=admin_operator.key.pem \
  --set-file secret.certs.files."fluentd\.pem"=fluentd.pem \
  --set-file secret.certs.files."fluentd\.key"=fluentd.key \
  --set-file secret.certs.files."bootstrapper\.key"=bootstrapper.key \
  --set-file secret.certs.files."certifier\.key"=certifier.key \
  --set-file secret.certs.files."certifier\.pem"=certifier.pem \
  --set-file secret.certs.files."nms_nginx\.key\.pem"=nms_nginx.key.pem \
  --set-file secret.certs.files."nms_nginx\.pem"=nms_nginx.pem \
  --set=docker.registry=docker.io \
  --set=docker.username=username \
  --set=docker.password=password |
kubectl apply -f -
```

