# Elastic Beanstalk - EB

- It is a platform as a service (PaaS) product in AWS, meaning the vendors handles all the infrastructure, we provide the code only
- EB is a developer focused product, providing managed application environments
- At a high level, developers provide code and EB handles infrastructure
- EB is fully customizable - uses AWS products under the covers provisioned with CloudFormation
- Using EB requires application support, does not come for free

## EB Platforms

- EB is capable of accepting code in many languages known as platforms
- EB has support for built-in languages, Docker and custom platforms
- Built-in supported languages: Go, Java SE, Java Tomcat, .NET Core (Linux) and .NET (Windows), Node.JS, PHP, Python, Ruby
- Docker options: single container docker and multicontainer docker (ECS)
- Preconfigured Docker: way to provide runtimes which are not yet natively supported, example Java with Glassfish
- We can create our own custom platform using packer which can be used with Beanstalk

## EB Terminology

- **Elastic Beanstalk Application**: is a collection of things relating to an application - a container/folder
- **Application Version**: specific labeled version of deployable code for an application. The source bundle is stored in S3
- **Environments**: are containers of infrastructure and configuration for a specific version
- Each environment is either a **web server tier** or a **worker tier**. The web server tier is designed to communicate with the end-users. The worker tier is designed to process work from the web tiers. Web server tier and worker tier communicate using SQS queues
- Each environment is running a specific version at any given time
- Each environment has its own CNAME, a CNAME SWAP can be done to exchange to environment DNS

## Deployment Policies

- **All at once**: 
    - Deploy to all instances at once
    - It is quick and simple, but it will cause a brief outage
    - Recommended for testing and development environments
- **Rolling**:
    - Application code is deployed in rolling batches
    - It is safer, since the deployment will continue only if the previous batch is healthy
    - The application will encounter loss in capacity
- **Rolling with additional batch**:
    - Same as rolling deployment, with the addition of having a new batch in order to maintain capacity during the deployment process
    - Recommended for production environment with real load
- **Immutable**:
    - New temporary ASG is created with the newer version of the application
    - Once the validation is complete, the older stack is removed
    - It is easier to roll back
- **Traffic Splitting**:
    - Fresh instances are created in a similar way as in case of immutable deployment
    - Traffic will be split between the older and the newer version
    - Allows to perform A/B testing on the application
    - It does not have capacity drops, but it will come with an additional cost
- **Blue/Green**:
    - Not automatically supported by EB
    - Requires manual CNAME swap between 2 environments
    - Provides full control in terms of when we would want to switch to the new environment

## EB and RDS

- In order to access an RDS instance from EB we can create an RDS instance within an EB environment
- If we do this, the RDS is linked to the environment
- If we delete the environment, the database will also be deleted
- If we link a database to an environment, we get access to the following environment properties:
    - `RDS_HOSTNAME`
    - `RDS_PORT`
    - `RDS_DB_NAME`
    - `RDS_USERNAME`
    - `RDS_PASSWORD`
- Other alternative is to create the RDS instance outside of the EB
- The environment properties above are not automatically provided in this case, we can create them manually
- With this method the RDS lifecycle is not tied to the EB environment
- Decoupling an existing RDS from an EB environment:
    1. Create a Snapshot
    2. Enable Delete Protection
    3. Create a new EB environment with the same app version
    4. Ensure new environment can connect to the DB
    5. Swap environment (CNAME or DNS)
    6. Terminate the old environment -  this will try to terminate the RDS instance
    7. Locate the `DELETE_FAILED` stack in CFN, manually delete the stack and pick to retain stuck resources

## Customizing via `.ebextensions`

- We can include directive to customize EB environments using `.ebextensions` folder
- Anything added in this folder as YAML or JSON format ending in `.config` is regarded to be configuration file
- This files has to be formatted as CloudFormation files
- EB will use CFN to create additional resources within the environment specified in the `.config` files
- This files can have also a number of config elements:
    - `option_settings`: allows us to set options for resources
    - `Resources`: allows us to create new resources using CFN elements
    - packages, sources, files, users, groups, commands, container_commands and services

## EB with HTTPS

- To use HTTPS with EB we need to apply an SSL certificate to the load balancer
- We can do this using the EB console or we can use the `.ebextensions/securelistener-[alb|nlb].config` feature
- We can configure the security group as well to allow SSL connections

## Environment Cloning

- Cloning allows to create new EB environment by cloning existing environments
- By cloning an environment we don't have to manually configure options, env. variables, resources and other settings
- A clone does copy any RDS instance defined, but the data is not copied by default
- EB cloning does not include any un-managed changes to resources from the environment
- To clone an environment from the eb command line we can use `eb clone <ENV>` command

## EB and Docker

### Single Container Mode

- We can only run one container in one Docker host
- This mode uses EC2 with Docker, not ECS
- In order to use this mode we have to provide a few configurations:
    - `Dockerfile`: used to create a new container image from this file
    - `Dockerrun.aws.json` (version 1): to use an existing docker image. We can configure ports, volumes and other Docker attributes
    - `Docker-compose.yml`: if we want to use Docker compose

### Multi-Container Mode

- Elastic Beanstalk uses ECS to create a cluster
- ECS uses EC2 instances provisioned in the cluster and an ELB for HA
- EB takes care of ECS tasks, cluster creation, task definition and task execution
- We need to provide an `Dockerrun.aws.json` (version 2) file in the application source bundle (root level)
- Any images need to be stored in a container registry such as ECR