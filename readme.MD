# Ingress Controller

1
```
eksctl utils associate-iam-oidc-provider \
--region us-east-1 \
--cluster expense \
--approve
```

2
```
curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.11.0/docs/install/iam_policy.json
```

3
```
aws iam create-policy \
--policy-name AWSLoadBalancerControllerIAMPolicy \
--policy-document file://iam-policy.json
```

4
```
eksctl create iamserviceaccount \
--cluster=expense \
--namespace=kube-system \
--name=aws-load-balancer-controller \
--attach-policy-arn=arn:aws:iam::533267158693:policy/AWSLoadBalancerControllerIAMPolicy \
--override-existing-serviceaccounts \
--region us-east-1 \
--approve
```

5
```
helm repo add eks https://aws.github.io/eks-charts
```

6
```
helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=expense --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
```