1.  Create an IAM OIDC provider:
    ```
    eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster robomart-dev \
    --approve
    ```

2.  Create IAM Policy:
    ```
    curl -o iam-policy.json https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.17.0/docs/install/iam_policy.json
    ```