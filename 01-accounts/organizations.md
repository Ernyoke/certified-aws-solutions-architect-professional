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

## Best Practices

- Have a single account into which users can log into and assume IAM roles in order to access other accounts from the org
- The account with all the identities may be the Management Account or it can be another Member Account (*Login Account*)

## `OrganizationAccountAccessRole`

- This is an IAM role used to access the newly added/created account in an organization
- This role will be created automatically if we create the account from an existing organization
- This role has to be created manually in the member account if the account was invited into the organization

# Service Control Policies (SCP)

- They are a feature of AWS Organizations used to restrict AWS accounts
- They are JSON documents
- They can be attached to the root of the organization, to one or more OUs or to individual AWS accounts
- SCPs inherit down through the organization tree
- The Management Account is special: even if it has SCPs attached (directly or through an OU) it wont be affected by the SCP
- SCPs are account permission boundaries:
    - They limit what the account (including the root user of the account) can do
    - We can never restrict a root user from an account, but we can restrict the account itself, hence these restrictions will apply to the root user as well
- **SCPs don't grant any permissions!** This are just a boundary to limit what is and is not allowed in an account
- SCPs can be used in two ways:
    - Deny list (default): allow by default and block access to certain services
        - `FullAWSAccess`: policy applied by default to the org an all OUs when we enable SCPs. This policy means tha by default nothing is restricted
        - SCPs don't grant permissions, but when they are enabled, there is a default deny for everything. This is why the `FullAWSAccess` policy is needed
        - SCP priority rules:
            1. Explicit Deny
            2. Allow
            3. Default (implicit) deny
        - Benefits of deny lists is that as AWS is extends the list of service offerings, new services will be available for accounts (low admin overhead)
    - Allow list: block by default and allow certain services
        - To implement allow lists:
            1. Remove the `FullAWSAccess` policy
            2. Add any services which should be allowed in a new policy
        - Allow lists are more secure, but they require more admin overhead