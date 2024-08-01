# AWS Security Hub

- Provides a comprehensive view of our security state in AWS
- Helps us assess our AWS environment against security industry standards and best practices
- Security Hub collects security data across AWS accounts, AWS services, and supported third-party products and helps us analyze our security trends and identify the highest priority security issues
- Security Hub needs to be enabled to be used. There are two ways to enable AWS Security Hub, by integrating with AWS Organizations or manually
- Organization can have a delegated administrator for the Security Hub

## Central Configuration

- It is a feature that helps us set up and manage Security Hub across multiple AWS accounts and regions
- We must integrate Security Hub and Organizations to use central configuration. Also, we must designate a home Region to use central configuration. The home Region is the Region from which the delegated administrator configures the organization
- The delegated administrator account can create AWS Security Hub configuration policies to configure Security Hub, security standards, and security controls in your organization
- Policy configurations:
    - Configuration policies must be associated to take effect
    - An account or OU can be associated with only one configuration policy
    - Configuration policies are complete: Configuration policies provide a complete specification of settings
    - Configuration policies can't be reverted: There's no option to revert a configuration policy after you associate it with accounts or OUs
    - Configuration policies take effect in your home Region and all linked Regions
    - Configuration policies are resources: they have an ARN
- Types of configuration polices:
    - Recommended configuration policy - provided by AWS
    - Custom configuration policy