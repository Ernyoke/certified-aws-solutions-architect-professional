# AWS Certificate Manager - ACM

- HTTPS (SSL/TSL) was designed to address security problems occurred with HTTP
- HTTPS provides data encryption in-transit and certificates to prove the identity
- ACM can function as a public certificate authority or a private certificate authority (CA)
- In case of a private CA applications need to be configured to trust the private CA
- With ACM we can generate or import certificates
- If ACM generates the certificate, it can renew it automatically. If imported, the user is responsible for renewal <span style="color: red;">EXAM</span>
- ACM can only deploy certificates to supported services (services in AWS which are integrated with ACM)
- Not all services all supported. Services which integrate with ACM are the following: load balancers, CloudFront, Cognito, Elastic Beanstalk, App Runner, API Gateway, AWS Nitro Enclaves, OpenSearch, AWS Network Firewall. EC2, for example, is not supported <span style="color: red;">EXAM</span>
- ACM is a regional service
- Certificates cannot leave the region they are generated or imported in, to use a certificate within an ALB in ap-southeast-2, the certificate needs to be in ACM in ap-southeast-2 (ACM is a regional service!!!) <span style="color: red;">EXAM</span>
- For global Services such as CloudFront, certificates should be stored in **us-east-1** !