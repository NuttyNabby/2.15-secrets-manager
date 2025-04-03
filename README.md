# 2.15-secrets-manager
1. What is needed to authorize your EC2 to retrieve secrets from the AWS Secret Manager?
To allow an EC2 instance to retrieve secrets from AWS Secrets Manager, you need:

An IAM Role with an attached policy that grants the necessary permissions.

Attach the IAM Role to the EC2 instance.

Ensure the EC2 instance has the necessary AWS SDK or CLI tools to retrieve secrets.

Use the AWS STS credentials (Instance Profile) within the EC2 instance to access Secrets Manager.

Steps:

- Create an IAM Role:

- Go to AWS IAM → Roles → Create role.

- Select AWS service → EC2.

- Attach a policy (either AWS managed or custom) that allows access to Secrets Manager.

- Assign the role to your EC2 instance.

- Attach the IAM Role to the EC2 Instance:

- Go to EC2 console → Select your instance.

- Click Actions → Security → Modify IAM role.

- Select the IAM role you created and attach it.

- Use AWS CLI or SDK to retrieve the secret:

#use this code :
aws secretsmanager get-secret-value --secret-id prod/cart-service/credentials --region us-east-1

2. Deriving the IAM policy for AWS Secrets Manager
To allow an EC2 instance to retrieve secrets from Secrets Manager, we define the following IAM policy:

# add this json:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "secretsmanager:GetSecretValue",
                "secretsmanager:DescribeSecret"
            ],
            "Resource": "arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/cart-service/credentials-*"
        }
    ]
}

#Breaking Down the IAM Policy:
Effect: "Allow" → Grants permission.

Action:

"secretsmanager:GetSecretValue" → Allows retrieval of the secret.

"secretsmanager:DescribeSecret" → Allows reading metadata of the secret.

Resource:

#Breaking Down the ARN format for a Secrets Manager secret:

arn:aws:secretsmanager:<region>:<account-id>:secret:<secret-name>*
Example ARN for the given secret (prod/cart-service/credentials):

arn:aws:secretsmanager:us-east-1:123456789012:secret:prod/cart-service/credentials-*
The wildcard * at the end allows access to all versions of the secret.
