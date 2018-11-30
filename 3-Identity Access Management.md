# 3. Identity Access Management

## 3.1 Introduction

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

## 3.2 Terminology

- Users: AWS User
- Groups: Group of users with the same permissions
- Roles: An IAM role is similar to a user, in that it is an AWS identity with permission policies that determine what the identity can and cannot do in AWS. However, instead of being uniquely associated with one person, a role is intended to be assumable by anyone who needs it
- Policy: A set of permissions. Policy Documents are written in JSON. 

## 3.3 Region

- IAM Users, groups, roles, permissions and policies are global; they are not specific to any region or availability zone.

## 3.4 User Management

### 3.4.1 Root User

- The root account is the account that you used to sign up on AWS.
- Root user has AdministratorAccess (Access to all AWS services).
- PowerUserAccess: Access to all AWS services except for management of groups and users within IAM 
- Always setup Multifactor authentication on root account.
- can customize Password rotation policies

### 3.4.2. New Users

- New users have no permissions when created

- New users are assigned Access Key ID & Secret Access Key when created.
- Access Key ID & Secret Access Key are credentials used to access AWS via API or CLI.
- Secrety Access Key is only visible once when the user is created. If the key is lost, or needs to be regenerated - it can be done by clicking on the user, selecting the "Security Credentials tab" and hitting "Create Access Key".

- New users are not assigned a password so they can not access the AWS console.
- The root user must generate a password for the user so that he/she may access the AWS console.

- When setting up an AWS account, the root user should create users, put those users into groups and assign permissions/policies to those groups. Groups could be e.g. HR, Accounting etc.

- Permissions can be attach to a user via groups and individually.

## 3.5 Roles

IAM roles are a secure way to grant permissions to entities that you trust

e.g.

- IAM user in another account
- Application code running on an EC2 instance that needs to perform actions on AWS resources
- An AWS service that needs to act on resources in your account to provide its features


## 3.6 Policy Documents

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

## 4.0 Quiz

1. Which statement best describes IAM

- IAM allows you to manage users, groups, roles, and their corresponding level of access to the AWS Platform.

- IAM allows you to manage users passwords only. AWS staff must create new users for your organisation. This is done by raising a ticket.

- IAM allows you to manage permissions for AWS resources only.

- IAM stands for Improvised Application Management and it allows you to deploy and manage applications in the AWS Cloud.

2. Power User Access allows ________.

- Full Access to all AWS services and resources

- Read Only access to all AWS services and resources

- Access to all AWS services except for management of groups and users within IAM

- Users to inspect the source code of the AWS platform

3. What level of access does the "root" account have?

- ReadOnlyAccess
- PowerUserAccess
- AdministratorAccess
- NoAccess

3. What level of access does a new user have?

- ReadOnlyAccess
- PowerUserAccess
- AdministratorAccess
- NoAccess

4. Access Key ID & Secret Access Key used for?

- Loging into AWS Console
- Creating an EC2 instance
- Encrypting S3 files
- Communicating with AWS API or CLI

5. What is an additional way to secure IAM for both the root login and new users alike?

- Implement multi-factor Authentication for all accounts. 
- Store the access key id and secret access key of all users in a publically accessible plain text document on S3 of which only you and members of your organisation know the address to.
- Configure the AWS console so that you can only log in to it from a specific IP Address range
- Configure the AWS console so that you can only log in to it from your internal network IP address range.

6. You are a solutions architect working for a large engineering company who are moving their existing legacy hardware to AWS. You have configured their first AWS account and you have set up IAM. Your company will be primarily based out of West Germany, however they will have a small subsidiary operating out of South Korea and you will need an AWS environment configured there as well. Which of the following statements is true

- You will then need to configure Users and Policy Documents for each region respectively.
- You will need to configure Users and Policy Documents only once, as these are applied globally.
- You will need to configure your users regionally, however your policy documents are global.
- You will need to configure your policy documents regionally, however your users are global.

7. You have a client who is considering moving to AWS services and do not yet have an account. What is the first thing the company should do to set up an AWS Account?

- Set up an account using Cloud Search.
- Set up an account using their company email address.
- Set up an account via SQS (Simple Queue Service)
- Set up an account via SNS (Simple Notification Service)

8. You are a security administrator working for a hotel chain. You have a new member of staff who has started as a systems administrator and they will need full access to the AWS console. You have created the user account and generated the access key id and the secret access key. You have moved this user into the group where the other administrators are and you have provided the new user with their secret access key and their access key id. However when they go to log in to the AWS console, they cannot sign in. What could be the cause of this?

- You have not applied the "log in from console" policy document to the user. You must apply this first so that they can log in.
- You have not applied the "log in from console" policy document to the user. You must apply this first so that they can log in.
- You have not yet activated multi-factor authentication for the user, so by default they will not be able to log in.
- You cannot log in to the AWS console using the Access Key ID and Secret Access Key, instead you must generate a password for the user and supply the user with this password, as well as the unique link to sign in to the AWS console. 

9. By default when you create a new user in the IAM console, what level of access do they have?

- Read Only Access to all AWS services
- No access to all AWS services
- Administrator access to all AWS Services
- Power USer access to all aws services

10. In what language are policy documents written in?

- Node.js
- Java
- Python
- JSON