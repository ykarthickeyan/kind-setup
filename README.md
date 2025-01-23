# kind-setup cluster setup for windows
# The folders have different kind configurations for creating clusters on the go.
https://kind.sigs.k8s.io/docs/user/quick-start/#installation --> Install kind depending on your OS
Kubernetes local Windows setup with KIND Multi-cluster
Added Ports 30950 for mapping wih localhost 30950 for testing node port

> [!NOTE]
> Metrics server needs to run seperately 
> Without CNI kind cluster needs Calico or other CNI to be installed to ensure the cluster nodes start properly. If not it will not start

# Command to create cluster
kind create cluster --config kind-cluster-config.yaml --name kube-test-cluster

### Deployment and service test
kubectl apply -f dep1.yaml

kubectl expose deploy/myapp --port=80 --target-port=80 --name=myapp

#### test the above service as a cluster ip
kubectl run tmp --image=busybox --restart=Never -i -- wget -O- myapp:80

kubectl edit svc myapp -> change type to nodeport and ndodeport value to 30001 as this port mapped to Kind cluster on local

Test using node port in general
wget -O- <nodeip>:30001

For testing on Kind cluster 
wget -O- localhost:30001

