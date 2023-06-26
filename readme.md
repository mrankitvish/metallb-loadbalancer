## Deploy MetalLB LoadBalancer on Kubernetes

---
##### Description:
MetalLB is a load balancer for Kubernetes designed for bare metal clusters. It provides a software-based load balancer implementation for Kubernetes services of type "LoadBalancer". MetalLB allows you to define a pool of IP addresses that can be used as the external IP addresses for Kubernetes services. It supports layer 2 and BGP mode for announcing the allocated IP addresses.

##### Pre-requisites:
- Kubectl and Helm must be installed and configured to work with your Kubernetes cluster. This will allow you to interact with your cluster and install MetalLB using Helm.
- You should have a range of IP addresses available that you can assign to MetalLB. These IP addresses will be used to provide external IP addresses for Kubernetes services of type "LoadBalancer".
---

##### Installing MetalLB:
1. Loging to your administrator machine (where kubectl and Helm installed)
2. Adding repo on Helm
```bash
# helm repo add metallb https://metallb.github.io/metallb
```
3. Installing metallb
```bash
# helm install metallb metallb/metallb
```
###### `Note`: 
The `helm install` command installs MetalLB in the `default` namespace, but you can install it in a specific namespace using the `--namespace` flag followed by the namespace name. For example, `helm install metallb metallb/metallb --namespace="metallb-system" --create-namespace` installs MetalLB in the `metallb-system` namespace. The `--create-namespace` flag creates the namespace if it does not already exist.

##### Configuring MetalLB:
1. Create a configmap.yaml file
```bash
# vim configmap.yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: first-pool
  namespace: metallb-system
spec:
  addresses:
  - 10.0.0.100-10.0.0.250 # Replace with yours IP address range

---

apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: example
  namespace: metallb-system

```
2. Save the file
3. Apply configmap.yaml file
```bash
# kubectl apply -f configmap.yaml
```