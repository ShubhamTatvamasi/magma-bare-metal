# agw bare metal

We should have enp1s0 and enp2s0 network interface, check by running:
```bash
ip a
```

As root user Install AGW:
```bash
bash su
wget https://raw.githubusercontent.com/magma/magma/v1.6/lte/gateway/deploy/agw_install_ubuntu.sh
bash agw_install_ubuntu.sh
```
> The machine will reboot but It's not finished yet.

The script is still running in the background. You can follow the output there:
```bash
journalctl -fu agw_installation
```

When you see `AGW installation is done.` It means that your AGW installation is done, you can make sure magma is running by executing:
```bash
service magma@* status
```

### Access Gateway Configuration

First, copy the root CA for your Orchestrator deployment into your AGW:
```bash
scp rootCA.pem magma@10.0.2.1:~
ssh magma@10.0.2.1
```

Move  root CA to right path:
```bash
sudo mkdir -p /var/opt/magma/tmp/certs/
sudo mv rootCA.pem /var/opt/magma/tmp/certs/rootCA.pem
```

Then, point your AGW to your Orchestrator:
```bash
sudo mkdir -p /var/opt/magma/configs
cd /var/opt/magma/configs
sudo vi control_proxy.yml
```

Put the following contents into the file:
```yaml
cloud_address: controller.yourdomain.com
cloud_port: 443
bootstrap_address: bootstrapper-controller.yourdomain.com
bootstrap_port: 443
fluentd_address: fluentd.yourdomain.com
fluentd_port: 24224

rootca_cert: /var/opt/magma/tmp/certs/rootCA.pem
```

Then restart your services to pick up the config changes:
```bash
sudo service magma@* stop
sudo service magma@magmad restart
```
