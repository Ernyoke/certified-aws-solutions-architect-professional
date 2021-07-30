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

## SSM Run Command

- It allows us to run commands on managed instances
- It takes a Command Document and executes it using the agent installed on the instance
- It does this without using SSH/RDP protocol
- Command documents can be executed on individual instances, multiple instances based on tags or resource groups
- Command documents can be reused and they can have input parameters
- Rate Control: if we are running commands on lot of instances, we can control it by using rate control. It can be based on:
    - Concurrency: on how many instances must run the command at a time
    - Error Threshold: defines how many individual commands can fail
- Output of commands can be sent to S3 or we can send SNS notifications
- Commands can be integrated with EventBridge

## SSM Patch Manager

- Allows to patch Linux and Windows instances running in EC2 or on-premises
- Concepts:
    - **Patch Baseline**: we can have many of these defined. Defines what should be installed (what patches, what hot-fixes)
    - **Patch Groups**: what resources we want to patch
    - **Maintenance Windows**: time slots when patches can take place
    - **Run Command**: base level functionality to perform the patching process. The command used for patching is `AWS-RunPatchbaseline`
    - **Concurrency** and **Error Threshold**: (see above)
    - **Compliance**: after patches are applied, system manager can check success of compliance compared to what is expected
- Patch Baselines:
    - For Linux: `AWS-[OS]DefaultPatchBaseline` - explicitly defines patches, example: `AWS-AmazonLinux2DefaultPatchBaseline`, `AWS-UbuntuDefaultPatchBaseline` - contain security updates and any critical update
    - For Windows: `AWS-DefaultPatchBaseline`, `AWS-WindowsPredefinedPatchBaseline-OS`, `AWS-WindowsPredefinedPatchBaseline-OS-Application`

