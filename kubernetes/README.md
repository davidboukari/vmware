## Set config.yaml

Sample [config.yaml](config.yaml)

## Initialize a kube cluster
```bash
tkg init --infrastructure vsphere --vsphere-controlplane-endpoint-ip 192.168.0.155
tkg init --infrastructure vsphere --name vsphere-management-cluster --plan dev --vsphere-controlplane-endpoint-ip 192.168.0.155 --deploy-tkg-on-vSphere7
```

## ssh connexion
```bash
ssh capv@<ip>
```
## Get the logs
```bash
kubectl logs deployment.apps/capv-controller-manager -n capv-system manager --kubeconfig  /root/.kube-tkg/tmp/config_WtjLHBIU
```

## Delete
kind get clusters | xargs -n1 kind delete cluster --name
