# CloudHSM

- Similar to KSM, it creates, manages and secures cryptographic material or keys
- KMS is a shared service. AWS has a certain level of access to the product, they manage the hardware and the software of the system
- KMS uses behind the scene HSM devices
- CloudHSM is true single tenant HSM hosted by AWS
- AWS provisions the hardware for CloudHSM but they do not have access to it. In case of losing access to a HSM device there is no easy way to re-gain the access to it
- CloudHSM is fully compliant with FIPS 140-2 Level 3 (KMS is L2 compliant overall)
- CloudHSM is accessed with industry standards APIs: PKCS#11, Java Cryptography Extensions (JCE), Microsoft CryptoNG (CNG) libraries
- KMS can use CloudHSM as a custom key store, CloudHSM integration with KMS

## CloudHSM Architecture

- CloudHSM devices are deployed into a VPC managed by AWS
- They are injected into customer managed VPCs using ENIs (Elastic Network Interfaces)
- For HA we need to deploy multiple HSM devices and configure them as a cluster
- A client needs to be installed on the EC2 instances in order to be able to access HSM modules
- While AWS do provision the HSM devices, we as customers are responsible for the management of the customer keys
- AWS can provide software updates on the HSM devices, but these should not affect the encryption storage part

## CloudHSM Use Cases

- There is no native integration with AWS service, this means CloudHSM can not be used for S3 SSE
- CloudHSM can be used for client-side encryption before uploading data to S3
- CloudHSM can be used to offload SSL/TLS processing for web servers
- Oracle Databases from RDS can perform Transparent Data Encryption (TDE) using CloudHSM
- CloudHSM can be used to protect private keys for an Issuing Certificate Authority (CA)