## set config.yaml

## initialize a kube cluster
tkg init --infrastructure vsphere --vsphere-controlplane-endpoint-ip 192.168.0.155

## Delete
kind get clusters | xargs -n1 kind delete cluster --name
