# AWS Organizations

- Standard AWS account: it is an account which is not in an AWS Organization
- We create an AWS Organization from a standard AWS account
- The organization is not created in this account, we just use the account to create the organization. The standard account then becomes the **Management Account** (used to be called *Master Account*)
- Using the Management Account we can invite other accounts into the organization
- When a standard account joins an organization, it will change to **Member Account** of that organization
- Organizations have 1 Management Account and 0 or more Member Accounts
- We can create a structure of AWS accounts in an organization. We can group accounts by things such as business units, function or development stage, etc.
- This structure is hierarchical, it is an inverted tree
- At the top of this tree is the root container of the organization (just a container within the organization, NOT to be confused with the root user)
- This root container can contain other containers, this containers are known as **Organizational Units (OU)**
- OUs can contains accounts (Management/Member accounts) or other OUs

## Consolidated Billing

- It is an important feature of AWS Organizations
- The individual billing method of each account from the organization is removed, the member accounts pass their billing through the Management Account (**Payer Account**)
- Using consolidated billing we get a single monthly bill. This covers the Management Account and all the Member Accounts of the Organization
- When using organization reservation benefits and discounts are pooled, meaning the organization can benefit as a whole for the spending of each AWS account within the org

## SCP - Service Control Policies

- Is a feature of AWS Organizations that lets us restrict what can accounts do in an organization

## Best Practices

- Have a single account into which users can log into and assume IAM roles in order to access other accounts from the org
- The account with all the identities may be the Management Account or it can be another Member Account (*Login Account*)