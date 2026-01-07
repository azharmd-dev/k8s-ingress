1.  Create an IAM OIDC provider:
    ```
    eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster robomart-dev \
    --approve
    ```