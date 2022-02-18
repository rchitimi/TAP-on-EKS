### Insall Tanzu CLIs

### Download and install Tanzu cluster essentials and tap

```
tar -xvf ~/Downloads/tanzu-cluster-essentials-darwin-amd64-1.0.0.tgz -C $HOME/tap/tanzu-cluster-essentials
export INSTALL_BUNDLE=registry.tanzu.vmware.com/tanzu-cluster-essentials/cluster-essentials-bundle@sha256:82dfaf70656b54dcba0d4def85ccae1578ff27054e7533d08320244af7fb0343
export INSTALL_REGISTRY_HOSTNAME=registry.tanzu.vmware.com
export INSTALL_REGISTRY_USERNAME=TANZU-NET-USER
export INSTALL_REGISTRY_PASSWORD=TANZU-NET-PASSWORD
./install.sh
```

tar -xvf ~/Downloads/tanzu-framework-darwin-amd64.tar -C $HOME/tanzu
export TANZU_CLI_NO_INIT=true
