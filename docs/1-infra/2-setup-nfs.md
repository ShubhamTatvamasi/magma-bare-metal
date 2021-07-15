# setup nfs

Install the following package on node:
```bash
sudo apt install -y nfs-common
```

Add helm stable repo:
```bash
helm repo add stable https://charts.helm.sh/stable/
helm repo update
```

Setup a Persistent Volume:
```bash
kubectl apply -f - << EOF
apiVersion: v1
kind: PersistentVolume
metadata:
  name: data-nfs-server-provisioner-0
spec:
  storageClassName: manual
  capacity:
    storage: 200Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /var/data/nfs
EOF
```

Install NFS Server Provisioner:
```bash
helm install nfs-server-provisioner stable/nfs-server-provisioner \
  --create-namespace \
  --namespace nfs-server-provisioner \
  --set image.tag=v2.3.0 \
  --set persistence.size=200Gi \
  --set persistence.enabled=true \
  --set persistence.storageClass=manual \
  --set storageClass.defaultClass=true
```
