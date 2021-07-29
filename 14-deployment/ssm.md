# AWS Systems Manager (SSM)

- Is a product which lets us manage and control AWS and on-premise infrastructure
- SSM is agent based, which means an agent needs to be installed on Windows and Linux based AMIs
- SSM manages inventory (what application are installed, files, network config, hw details, services, etc.) and can path assets
- It can also run commands and manage desired state of instances (example: block certain ports)
- It provides a parameters store for configurations and secrets
- Finally it provides session manager used to securely connect to EC2 instances even in private VPCs

## Agent Architecture

- An instances needs the SSM agent to be installed in order to be able to be managed by the service
- It also needs an EC2 instance role attached to it which allows communication with the service
- Instances require connectivity to the SSM service. This can be done via IGW or VPCE
- On the on-premises side we need to create managed instance activations
- For each activation we will receive an activation code and an activation ID
