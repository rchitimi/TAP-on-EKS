### Installing EKS

```
brew tap weaveworks/tap
brew install weaveworks/tap/eksctl
```

### Install EKS Cluster - Default is 2 nodes only: 
```
eksctl create cluster --name eks-tap-demo --region us-east-1 
eksctl get nodegroup --cluster=eks-tap-demo -r us-east-1
eksctl scale nodegroup --cluster=eks-tap-demo -r us-east-1 --nodes=6 --name=ng-55c39af3 --nodes-min=6  --nodes-max=10
```
