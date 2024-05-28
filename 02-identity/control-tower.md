# AWS Control Tower

- Control Tower's job is to allow quick and easy setup of a multi-account environment
- Control Tower orchestrates other AWS services to provide this functionality
- It is using Organizations, IAM Identity Center, CloudFormation, AWS Config and more
- It is essentially an evolution of AWS Organizations adding more features, intelligence and automation

## Control Tower Features

- **Landing Zones**: it is the multi-account environment
    - Provides via other AWS services:
        - SSO/ID Federation
        - Centralized logging and auditing
- **Guard Rails**: used to detect/mandate rules and standards across all accounts
- **Account Factory**: automates and standardizes new account creation
- **Dashboard**: single page oversight of the entire environment
- When Control Tower is set up, it creates to OUs:
    - Foundational OU (security)
    - Custom OU (sandbox)
- Inside the Foundational OU Control Tower creates two accounts:
    - Audit Account: for users who needs access to audit information provided by Control Tower
    - Log Archive Account: for users who need access to all log information to all enrolled accounts within the Landing Zones
- Within the Custom OU Account Factory will provision AWS accounts in an automated way
- For these new accounts we have bases templates for configurations:
    - Account Baseline template
    - Network Baseline template (cookie cutter)
- Control Tower utilizes CloudFormation under the hood to implement all of these automation
- It also uses both AWS Config and SCPs to implement account Guard Rails

## Landing Zones

- It is a feature designed that anyone should be able to implement a *well architected* multi-account environment
- Home Region: the region where we initially deploy the product
- A Landing Zone creates the Security and Sandbox OUs, we can create other OUs and accounts as well
- Landing Zones utilizes the IAM Identity Center for multiple-accounts and ID Federation
- A Landing Zone provides monitoring and notifications via CloudWatch and SNS
- We can allow end-users to provision new accounts from a Landing Zone using Service Catalog

## Guardrails

- They are rules for multi-account governance
- There are 3 different types of rules:
    - Mandatory
    - Strongly Recommended
    - Elective
- Guardrails function in two different ways:
    - Preventive: stop us from doing things
        - They are implement using SCPs
        - They are enforced or not enabled
        - Example: allow or deny regions; disallow bucket policy changes in the Org
    - Detective: compliance check
        - Uses AWS Config Rules to detect compliance violations
        - These types of Guardrails are either *clear*, *in violation* or *not enabled*
        - Example: check if CloudTrail is enabled; check if EC2 instances have public IPv4 attached

## Account Factory

- Allows automated account provisioning
- This can be done by either cloud administrators or end users (with appropriate permissions)
- Guardrails are automatically applied to the provisioned accounts
- Account admin can be given to a named user who provisions the account or to another user
- Accounts can be configured with standard account and network configurations
- Can be fully integrated with a businessess SDLC