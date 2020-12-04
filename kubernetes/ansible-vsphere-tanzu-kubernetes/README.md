# vsphere-tanzu-kubernetes

* https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-install-tkg-vsphere.html
* https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-install-tkg-vsphere-cli.html

## Installation 
* Create a directory DCname/tkg
* Deploy ova template 
. photon-3-kube-v1.19.1-vmware.2.ova
. photon-3-haproxy-v1.2.4-vmware.1.ova
* Convert them at template and copy them in the directory tkg
* Deploy a centos VM
* In the centos VM
```bash
yum install 
yum install epel-release vim git ansible
git clone git@github.com:davidboukari/vmware.git
cd vmware/kubernetes/ansible-vsphere-tanzu-kubernetes
ansible-playbook playbook.yml -vvv

tkg init --infrastructure vsphere --vsphere-controlplane-endpoint-ip 192.168.0.155
or
tkg init --infrastructure vsphere --name vsphere-management-cluster --plan dev --vsphere-controlplane-endpoint-ip 192.168.0.155 --deploy-tkg-on-vSphere7
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
