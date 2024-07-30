# Amazon Cognito

- Provides authentication, authorization and user management for mobile/web applications
- Provides 2 main pieces of functionality:
    - **User Pools**: used for sign-in, provides a JSON Web Token (JWT) after authentication
        - **User Pools do not grant access to AWS services!**
        - User Pools provide user directory management and user profiles, sign-up and sing-in with customizable web UI, MFA and other security features
        - They allow social sign-in provided by Google, Apple, Facebook as well as sign in using other identity types such as SAML identity providers

<img width="1312" alt="image" src="https://github.com/user-attachments/assets/03690f04-8f84-4585-bbcb-980ce7fd0714">

    - **Identity Pools**: the aim of identity pool is to exchange a type of external identity for temporary AWS credentials, which can be used to access AWS resources
        - Unauthenticated Identities: identity pools can provides access to AWS services for guest users (unauthenticated users)
        - Federated Identities: authorize users to temporarily access AWS resources after they authenticated with Google, Facebook, Twitter, SAML 2.0 and event Cognito User Pools

<img width="1312" alt="image" src="https://github.com/user-attachments/assets/6caff77a-6d75-4439-bc30-59c03a94a6ae">

## Cognito concepts

![Cognito Concepts](images/CognitoConcepts.png)

