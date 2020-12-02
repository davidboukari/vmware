## set config.yaml

Sample [config.yaml](config.yaml)

## initialize a kube cluster
```bash
tkg init --infrastructure vsphere --vsphere-controlplane-endpoint-ip 192.168.0.155
tkg init --infrastructure vsphere --name vsphere-management-cluster --plan dev --vsphere-controlplane-endpoint-ip 192.168.0.155 --deploy-tkg-on-vSphere7
```

## ssh connexion
```bash
ssh capv@<ip>
```

## Delete
kind get clusters | xargs -n1 kind delete cluster --name
