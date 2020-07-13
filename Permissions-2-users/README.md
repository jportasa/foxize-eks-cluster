# RBAC, asign permissions to AWS users to certain parts of the k8s cluster

Docu: 

https://aws.amazon.com/premiumsupport/knowledge-center/amazon-eks-cluster-access/
https://kubernetes.io/docs/reference/access-authn-authz/rbac/

Add the users in the mapRoles section:

Cual es mi identidad?
```
aws sts get-caller-identity
```
Edito el configmap aws-auth, sección mapUsers
```
kubectl edit configmap/aws-auth -n kube-system
```

```
apiVersion: v1 
kind: ConfigMap 
metadata: 
  name: aws-auth 
  namespace: kube-system 
data: 
  mapRoles: | 
    - rolearn: arn:aws:iam::11122223333:role/EKS-Worker-NodeInstanceRole-1I00GBC9U4U7B 
      username: system:node:{{EC2PrivateDNSName}} 
      groups: 
        - system:bootstrappers 
        - system:nodes 
  mapUsers: | 
    - userarn: arn:aws:iam::11122223333:user/designated_user 
      username: designated_user 
      groups: 
        - system:masters  # grupo max privilegios dentro de k8s, más info: https://kubernetes.io/docs/reference/access-authn-authz/rbac/
```
