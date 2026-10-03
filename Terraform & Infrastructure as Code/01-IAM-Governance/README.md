# AWS IAM - Governance

AWS Identity and Access Management (IAM) controls **who** can access AWS resources and **what** they can do. IAM is a global service: identities and policies are available across AWS Regions, although permissions still apply to resources in specific Regions.

## Core concepts

### Users

An IAM user represents a person or application that needs long-term credentials in an AWS account. A user can have a password for the AWS console and access keys for programmatic access.

Prefer federated workforce identities and temporary credentials over creating IAM users for employees. If an access key is unavoidable, rotate it, monitor its use, and never commit it to source control.

### Groups

An IAM group is a collection of users. Policies attached to a group apply to its members, which makes groups useful for job functions such as `Developers`, `Auditors`, or `ReadOnlyOperators`.

Groups cannot contain other groups and cannot be used as principals in resource-based policies. Attach shared permissions to groups rather than duplicating them on every user.

### Roles

An IAM role is an identity with permissions that can be assumed to receive temporary credentials. A role has:

- A **trust policy** that says who or what may assume it.
- One or more **permissions policies** that say what the role may do.

Roles are the normal way for EC2 instances, Lambda functions, containers, CI/CD systems, and federated users to access AWS without storing long-lived access keys.

### Policies

Policies are JSON documents that define permissions. A statement commonly contains:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::example-bucket/reports/*"
    }
  ]
}
```

Important policy elements include:

| Element | Purpose |
| --- | --- |
| `Effect` | `Allow` or `Deny` |
| `Action` | API operations, such as `s3:GetObject` |
| `Resource` | Resource ARNs to which the statement applies |
| `Principal` | Who is affected; mainly used in resource-based policies |
| `Condition` | Additional requirements, such as source VPC or MFA |

Policies may be identity-based, resource-based, permissions boundaries, session policies, SCPs in AWS Organizations, or ACLs for a small number of services. An explicit `Deny` overrides an `Allow`; otherwise, access is implicitly denied unless an applicable policy allows it.

### Permissions and least privilege

A permission is authorization to perform an action on a resource under specified conditions. **Least privilege** means granting only the actions, resources, and conditions required for a task, for only as long as required.

Prefer specific actions and resource ARNs to `Action: "*"` and `Resource: "*"`. Use IAM Access Analyzer, CloudTrail activity, and policy review to remove unused permissions. For short-lived administrative work, use temporary elevated access rather than permanently broad permissions.

## IAM best practices

1. Use IAM Identity Center or another federation provider for human access.
2. Require MFA, especially for the root user and privileged roles.
3. Do not use the root user for everyday work; secure its credentials and create an emergency access procedure.
4. Use roles and temporary credentials for workloads and CI/CD.
5. Apply least privilege and separate duties between development, deployment, audit, and production operations.
6. Centralize permissions through groups, roles, permission sets, and AWS Organizations SCPs.
7. Set a password policy and avoid shared users.
8. Rotate or remove unused access keys, and store unavoidable secrets in a managed secret store.
9. Review policies with IAM Access Analyzer and monitor activity with CloudTrail.
10. Test policies before production changes and document ownership of privileged roles.

## Common use cases

- Providing developers read-only access to a development account.
- Allowing a Lambda function to read from S3 and write logs to CloudWatch.
- Letting an EC2 instance access DynamoDB without embedding credentials.
- Giving a CI/CD role permission to deploy a specific application.
- Federating employees from an enterprise directory into multiple AWS accounts.
- Restricting production administration to approved roles and MFA-protected sessions.

## Useful distinction

Authentication answers **“Who are you?”**. IAM authorization answers **“What are you allowed to do?”**. IAM does not replace network controls such as security groups or encryption controls such as KMS.

## Further reading

- [AWS IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)
- [IAM policy elements](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)
- [IAM best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

