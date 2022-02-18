#### Deploy workload
```
kubectl create ns developer
```

#### Add secret to dev namespace
```
tanzu secret registry add registry-credentials --server harbor.tap.mytanzudemos.com --username admin --password “xxxxxxxx” --namespace developer
```

#### Add Roles, service account
```
cat <<EOF | kubectl -n developer apply -f -                                                           
apiVersion: v1
kind: Secret                           
metadata:                     
  name: tap-registry
  annotations:                             
    secretgen.carvel.dev/image-pull-secret: ""
type: kubernetes.io/dockerconfigjson
data:                   
  .dockerconfigjson: e30K             
---           
apiVersion: v1         
kind: ServiceAccount 
metadata:     
  name: default                                 
secrets:                 
  - name: registry-credentials
imagePullSecrets:
  - name: registry-credentials
  - name: tap-registry
---              
apiVersion: rbac.authorization.k8s.io/v1
kind: Role       
metadata:                
  name: default                      
rules:        
- apiGroups: [source.toolkit.fluxcd.io]
  resources: [gitrepositories]
  verbs: ['*']   
- apiGroups: [source.apps.tanzu.vmware.com]
  resources: [imagerepositories]
  verbs: ['*']
- apiGroups: [carto.run]          
  resources: [deliverables, runnables]
  verbs: ['*']
- apiGroups: [kpack.io]         
  resources: [images]           
  verbs: ['*']
- apiGroups: [conventions.apps.tanzu.vmware.com]
  resources: [podintents]      
  verbs: ['*']
- apiGroups: [""]                            
  resources: ['configmaps']               
  verbs: ['*']
- apiGroups: [""]
  resources: ['pods']                   
  verbs: ['list']
- apiGroups: [tekton.dev]
  resources: [taskruns, pipelineruns]
  verbs: ['*']
- apiGroups: [tekton.dev]            
  resources: [pipelines]
  verbs: ['list']
- apiGroups: [kappctrl.k14s.io]
  resources: [apps]     
  verbs: ['*']   
- apiGroups: [serving.knative.dev]                    
```

#### Deploy workload
```
tanzu apps workload create tanzu-java-web-app \                                 
--git-repo https://github.com/rchitimi/tanzu-java-web-app.git \
--git-branch main \
--type web \
--label app.kubernetes.io/part-of=tanzu-java-web-app \
--yes -n developer
```
#### Tail logs to see build output
```
tanzu apps workload tail tanzu-java-web-app --since 10m --timestamp -n developer 
```

#### Get URL to access application
```
tanzu apps workload get tanzu-java-web-app -n developer
```
