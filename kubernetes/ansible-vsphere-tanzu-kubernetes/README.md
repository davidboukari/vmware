# vsphere-tanzu-kubernetes

* https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-install-tkg-vsphere.html
* https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-install-tkg-vsphere-cli.html

## Installation 
* Create a directory DCname/tkg
* Deploy ova template
```bash
photon-3-kube-v1.19.1-vmware.2.ova
photon-3-haproxy-v1.2.4-vmware.1.ova
```
* Convert them at template and copy them in the directory DCname/tkg
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

## Delete the cluster
kind get clusters | xargs -n1 kind delete cluster --name


## Manipulate the workload cluster for the applications installations
* https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-tanzu-k8s-clusters-create.html
```bash
tkg get management-cluster
 MANAGEMENT-CLUSTER-NAME       CONTEXT-NAME                                                 STATUS
 vsphere-management-cluster *  vsphere-management-cluster-admin@vsphere-management-cluster  Success

kubectl config use-context vsphere-management-cluster-admin@vsphere-management-cluster
Switched to context "vsphere-management-cluster-admin@vsphere-management-cluster".

kubectl get nodes
NAME                                              STATUS   ROLES    AGE   VERSION
vsphere-management-cluster-control-plane-stj76    Ready    master   28m   v1.19.1+vmware.2
vsphere-management-cluster-md-0-bcbd45f87-h5vvn   Ready    <none>   22m   v1.19.1+vmware.2

kubectl get nodes
NAME                                              STATUS   ROLES    AGE   VERSION
vsphere-management-cluster-control-plane-stj76    Ready    master   28m   v1.19.1+vmware.2
vsphere-management-cluster-md-0-bcbd45f87-h5vvn   Ready    <none>   22m   v1.19.1+vmware.2

kubectl get pods
No resources found in default namespace.

kubectl get namespaces
NAME                                STATUS   AGE
capi-kubeadm-bootstrap-system       Active   20m
capi-kubeadm-control-plane-system   Active   20m
```

### workload cluster
* https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-tanzu-k8s-clusters-create.html
```
# Cluster manager
tkg get management-cluster
kubectl config use-context vsphere-management-cluster-admin@vsphere-management-cluster

# Worload cluster
tkg create cluster my-dev-cluster --plan dev --worker-machine-count 3 --vsphere-controlplane-endpoint-ip 192.168.0.176 
tkg get cluster
tkg delete cluster my-dev-cluster

# Scaling
* https://docs.vmware.com/en/VMware-Tanzu-Kubernetes-Grid/1.1/vmware-tanzu-kubernetes-grid-11/GUID-tanzu-k8s-clusters-scale-cluster.html
tkg scale cluster cluster_name --controlplane-machine-count 5 --worker-machine-count 10

kubectl get pods -A

# auto completion
yum install bash-completion
echo 'source <(kubectl completion bash)' >> ~/.bashrc 

# Get & Set label for nodes
kubectl get nodes --show-labels

# Expose a service
### nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: www-api
spec:
  containers:
  - name: www
    image: nginx:1.12.2
    ports:
    - containerPort: 80
### nginx-svc.yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  selector:
    app: www-api
  type: ClusterIP
  ports:
  - port: 8080
    targetPort: 80

#### test in a container
kubectl run curl --image=radial/busyboxplus:curl -i --tty
  nslookup nginx

```

