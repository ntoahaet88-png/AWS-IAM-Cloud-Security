# AWS Cloud Security with IAM

## Project Overview
This project demonstrates how to implement least-privilege access control on AWS using IAM policies, EC2 instances, and tag-based environment isolation.

Built as part of my hands-on cloud security learning journey.

---

## Tools & Services Used
- AWS EC2
- AWS IAM (Users, Groups, Policies)
- IAM Policy Simulator
- JSON Policy Language

---

## What I Built
- Launched two EC2 instances tagged as `production` and `development`
- Wrote a custom JSON IAM policy that:
  - Allows full EC2 actions **only** on instances tagged `Env: development`
  - Allows read-only `Describe` actions across all instances
  - **Denies** tag creation and deletion on all instances
- Created an IAM user group for interns with the policy attached
- Tested policy enforcement in the live AWS console
- Validated permissions using the IAM Policy Simulator

---

## IAM Policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:*",
      "Resource": "*",
      "Condition": {
        "StringEquals": {
          "ec2:ResourceTag/Env": "development"
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": "ec2:Describe*",
      "Resource": "*"
    },
    {
      "Effect": "Deny",
      "Action": [
        "ec2:DeleteTags",
        "ec2:CreateTags"
      ],
      "Resource": "*"
    }
  ]
}
