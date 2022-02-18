### Install TAP

#### Create namespace
```
kubectl create ns tap-install
export INSTALL_REGISTRY_HOSTNAME=registry.tanzu.vmware.com
export INSTALL_REGISTRY_USERNAME=TANZU-NET-USER
export INSTALL_REGISTRY_PASSWORD=TANZU-NET-PASSWORD
```

#### Create secret
```
tanzu secret registry add tap-registry \
  --username ${INSTALL_REGISTRY_USERNAME} --password ${INSTALL_REGISTRY_PASSWORD} \
  --server ${INSTALL_REGISTRY_HOSTNAME} \
  --export-to-all-namespaces --yes --namespace tap-install
``` 

#### Add Tanzu Repo
```
tanzu package repository add tanzu-tap-repository \
  --url registry.tanzu.vmware.com/tanzu-application-platform/tap-packages:0.4.0 \
  --namespace tap-install
```  


#### List available packages and repos
```
tanzu package repository get tanzu-tap-repository --namespace tap-install
tanzu package available list --namespace tap-install
```
#### configure TAP values
vi tap-values.yml (setup configuration…)

#### Install TAP
```
tanzu package install tap --package-name tap.tanzu.vmware.com --version 1.0.0 --values-file tap-values.yml -n tap-install
````
#### setup DNS by copying external ip and mapping to your domain
```
kubectl get service envoy -n tanzu-system-ingress
```
