### OIDC and IAM Setup steps

1.  Create an IAM OIDC provider:
    ```
    eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster robomart-dev \
    --approve
    ```

2.  Download IAM Policy for other regions:
    ```
    curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.17.0/docs/install/iam_policy.json
    ```

3.  Create an IAM policy named 'AWSLoadBalancerControllerIAMPolicy':
    ```
    aws iam create-policy \
    --policy-name AWSLoadBalancerControllerIAMPolicy \
    --policy-document file://iam-policy.json
    ```

4.  Create an IAM role and Kubernetes ServiceAccount for the LBC:
    ```
    eksctl create iamserviceaccount \
    --cluster=robomart-dev \
    --namespace=kube-system \
    --name=aws-load-balancer-controller \
    --attach-policy-arn=arn:aws:iam::522534289017:policy/AWSLoadBalancerControllerIAMPolicy \
    --override-existing-serviceaccounts \
    --region us-east-1 \
    --approve
    ```

### Driver Installation steps:

1.  Install Helm:
    ```
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-4
    chmod 700 get_helm.sh
    ./get_helm.sh
    ```

2.  Add the EKS chart repo to Helm:
    ```
    helm repo add eks https://aws.github.io/eks-charts
    ```

3.  Install load balance controller:
    ```
    helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=robomart-dev --set serviceAccount.create=false --set serviceAccount.name=aws-load-balancer-controller
    ```