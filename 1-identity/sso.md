# AWS Single Sing-On - SSO

- A way to use existing enterprise identity store with AWS
- Allows to centrally manage SSO access to multiple AWS accounts and external business applications as well
- Replaces the historical uses cases provided by SAML 2.0
- Flexible Identity store: where identities are stored. Allows for external identities to be swapped with AWS credentials
- AWS SSO supports:
    - Built-in identity store
    - AWS Managed Microsoft AD
    - On-premise Microsoft AD
    - External Identity Provider - SAML 2.0
- AWS SSO is preferred by AWS for any "workforce" (enterprise) identity federation over the traditional SAML 2.0 based identity federation

## AWS SSO Architecture

![AWS SSO](images/AWSSSO.png)