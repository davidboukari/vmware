# vsphere-tanzu-kubernetes
```
pip install libselinux-python3
in pyvenv.cfg change  include-system-site-packages = true


parameters: 
- VSPHERE_xxx

Deactivate selinux
Install kubectl
Install tkg
Init tkg config
cp file/config.yaml .tkg/config.yaml
echo command to build cluster: tkg init --infrastructure vsphere --name vsphere-management-cluster --plan dev --vsphere-controlplane-endpoint-ip 192.168.0.155 --deploy-tkg-on-vSphere7
```

## 
```
Initialize a kube cluster

yum install docker
tkg init --infrastructure vsphere --vsphere-controlplane-endpoint-ip 192.168.0.155
tkg init --infrastructure vsphere --name vsphere-management-cluster --plan dev --vsphere-controlplane-endpoint-ip 192.168.0.155 --deploy-tkg-on-vSphere7

ssh connexion

ssh capv@<ip>

Get the logs

kubectl logs deployment.apps/capv-controller-manager -n capv-system manager --kubeconfig  /root/.kube-tkg/tmp/config_WtjLHBIU
```
