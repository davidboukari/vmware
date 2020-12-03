## Set config.yaml

Sample [config.yaml](config.yaml)

## Initialize a kube cluster
```bash
yum install docker
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

## 
```bash
tkg get management-cluster
 MANAGEMENT-CLUSTER-NAME       CONTEXT-NAME                                                 STATUS
 vsphere-management-cluster *  vsphere-management-cluster-admin@vsphere-management-cluster  Success
 
kubectl config use-context vsphere-management-cluster-admin@vsphere-management-cluster
Switched to context "vsphere-management-cluster-admin@vsphere-management-cluster".
 
 kuubectl get nodes
 kubectl get pods
 kubectl get namespaces


# Deploy a Cluster with Multiple Worker Nodes
tkg create cluster my-dev-cluster --plan dev --worker-machine-count 3 --vsphere-controlplane-endpoint-ip 192.168.0.129

# Deploy a Cluster in a Specific Namespace
tkg create cluster my-cluster --plan dev --namespace my_namespace

# Deploy a Kubernetes v1.17.3 cluster:
tkg create cluster k8s-1-17-3-cluster --plan dev --kubernetes-version v1.17.3

# Deploy a Kubernetes v1.17.6 cluster:
tkg create cluster k8s-1-17-6-cluster --plan dev  --kubernetes-version v1.17.6

# Deploy a Kubernetes v1.18.2 cluster:
tkg create cluster k8s-1-18-2-cluster --plan dev --kubernetes-version v1.18.2

 ```

## Delete
kind get clusters | xargs -n1 kind delete cluster --name
