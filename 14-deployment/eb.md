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