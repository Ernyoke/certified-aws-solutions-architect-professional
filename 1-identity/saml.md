# SAML 2.0 Identity Federation

- Federation lets user outside of AWS to assume a temporary role for access AWS resources
- These users assume identity provider access role
- SAML 2.0 - Security Assertion Markup Language
- SAML 2.0 allows to indirectly use on-premise IDs with AWS (console and CLI)
- SAML 2.0 based identity federation is used when we have and enterprise identity provider which is SAML 2.0 compatible
- SAML 2.0 based federation is ideal when we have an existing identity management team managing access to other services including AWS

## SAML 2.0 Identity Federation Authentication Process - API Access

![SAML 2.0 Federation API](images/SAML2.0FederationAPI.png)

## SAML 2.0 Identity Federation Authentication Process - AWS Console Access

![SAML 2.0 Federation Console](images/SAML2.0FederationConsole.png)

## SAML 2.0 Federation

- We need to setup trust between AWS IAM and SAML (both ways)
- SAML 2.0 enabled web based, cross domain SSO
- Uses the STS API: `AssumeRoleWithSAML`
- It is the old way of doing federation, recommended way by AWS is to use **Amazon Single Sign On (SSO)**