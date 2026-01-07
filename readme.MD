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




