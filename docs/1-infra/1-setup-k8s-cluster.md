# setup k8s cluster

Install docker:
```bash
sudo swapoff -a
curl https://releases.rancher.com/install-docker/20.10.sh | sh
```

Download rke:
```bash
sudo wget https://github.com/rancher/rke/releases/download/v1.2.9/rke_linux-amd64 \
  -O /usr/local/bin/rke
sudo chmod +x /usr/local/bin/rke
```

Generate config for RKE cluster:
```bash
rke config
```

Setup cluster:
```bash
rke up
```
