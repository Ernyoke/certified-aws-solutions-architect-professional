# AWS OpsWorks

- OpsWorks is configuration managed service which provides AWS managed implementation of Chef a Puppet
- OpsWorks functions in one of 3 modes:
    - Puppet Enterprise: we can create an AWS Managed Puppet Master Server (desired state architecture)
    - Chef Automate: we can create AWS Managed Chef Servers (similar as IaC, set of steps using Ruby)
    - OpsWorks: AWS implementation of Chef, no servers. Chef at a basic level, little admin overhead
- Generally we should only chose to use them if we are required to use Chef or Puppet, for example in case of a migration
- Other use case would be a requirement to automate

## Opsworks Mode

- **Stacks**: core components of OpsWorks, container of resource similar to CFN stacks
- **Layers**: represent a specific function in a stack, example layer of load balancers, layer of database, layer of EC2 instances running a web application
- **Recipes** and **Cookbooks**: they are applied to layers. We can use them to install packages, deploy applications, run scripts, perform reconfigurations. Cookbooks are collections of recipes which can be stored on GitHub
- **Lifecycle Events**: special events which run on a layer, examples:
    - Setup
    - Configure: generally executed when instances are removed or added to the stack. It will run on all instances, including already existing ones
    - Deploy
    - Undeploy
    - Shutdown
- **Instances**: compute instances (EC2 instances or on-premise servers). They can be:
    - **24/7** instances: started manually
    - **Time-Based** instances: configured to start and stop on a schedule
    - **Load-Based** instances: turn on or off based on system metrics (similar to ASG)
- Instance **auto-healing**: Opsworks automatically restarts instances in they fail for some reason
- **Apps**: they can be stored in repositories such as S3. Each app is represented by an OpsWorks App which specifies the application type and containing any information needed to deploy the app
- OpsWorks architecture:
    ![OpsWorks architecture](images/AWSOpsWorks.png)