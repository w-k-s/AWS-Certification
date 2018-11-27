#3. Identity Access Management

##3.1 Introduction

- Manage users and their level of access to the AWS console.

It provides: 
 
- Centralized control of Amazon account
- Shared access to AWS account
- Granular permissions
- Identity Federations (fetch identities from external directories e.g. LinkedIn, Facebook, ActiveDirectory)
- Multifactor Authentication
- Temporary access for users/devices and services
- Password rotation policy
- Integrates with many AWS services
- PCI DSS compliant

##3.2 Terminology

- Users: AWS User
- Groups: Group of users with the same permissions
- Roles: An IAM role is similar to a user, in that it is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it
- Policy: A set of permissions. Policy Documents are written in JSON. 

##3.3 Region

- IAM Users, groups, roles, permissions and policies are global; they are not specific to any region or availability zone.

##3.4 User Management

###3.4.1 Root User

- The root account is the account that you used to sign up on AWS.
- Root user has AdministratorAccess (Access to all AWS services).
- PowerUserAccess: Access to all AWS services except for management of groups and users within IAM 
- Always setup Multifactor authentication on root account.
- can customize Password rotation policies

###3.4.2. New Users

- New users have no permissions when created

- New users are assigned Access Key ID & Secret Access Key when created.
- Access Key ID & Secret Access Key are credentials used to access AWS via API or CLI.
- Secrety Access Key is only visible once when the user is created. If the key is lost, or needs to be regenerated - it can be done by clicking on the user, selecting the "Security Credentials tab" and hitting "Create Access Key".

- New users are not assigned a password so they can not access the AWS console.
- The root user must generate a password for the user so that he/she may access the AWS console.

- When setting up an AWS account, the root user should create users, put those users into groups and assign permissions/policies to those groups. Groups could be e.g. HR, Accounting etc.

- Permissions can be attach to a user via groups and individually.

##3.5 Roles

IAM roles are a secure way to grant permissions to entities that you trust

e.g.

- IAM user in another account
- Application code running on an EC2 instance that needs to perform actions on AWS resources
- An AWS service that needs to act on resources in your account to provide its features


##3.6 Policy Documents

- Policy Documents list permission in JSON format
- For example, the policy document of a PowerUser is:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "NotAction": [
                "iam:*",
                "organizations:*"
            ],
            "Resource": "*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "iam:CreateServiceLinkedRole",
                "iam:DeleteServiceLinkedRole",
                "iam:ListRoles",
                "organizations:DescribeOrganization"
            ],
            "Resource": "*"
        }
    ]
}
```