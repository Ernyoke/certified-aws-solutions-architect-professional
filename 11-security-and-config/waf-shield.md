# AWS Shield and Web Application Firewall (WAF)

## AWS Shield

- Provides protection against DDoS attacks
- Provides a custom designed set of protection against DDoS attacks
- Comes in 2 versions:
    - Shield Standard:
        - Offers protection against Layer 3 and Layer 4 DDoS attacks
        - It is free but for full its full potential with Route53 and CloudFront
    - Shield Advanced:
        - Costs $3000 per months
        - Expands the range of products which can be protected: EC2, ELB, CloudFront, Global Accelerator and Route53
        - Shield Advanced provides access to 24/7 advanced response team
        - Provides financial insurance for any increase of payments in case of DDoS attacks

## WAF - Web Application Firewall

- It is Layer 7 Firewall (understands HTTP/S)
- Normally firewall operate at Layer 3, 4, 5
- WAF protects against complex Layer 7 attacks/exploits such as SQL Injection, Cross-Site Scripting, Geo Blocks, Rate Awareness
- Web Access Control List (WEBACL) integrated with ALB, API Gateway and CloudFront
- WEBACL has rules and they are evaluated when traffic arrives