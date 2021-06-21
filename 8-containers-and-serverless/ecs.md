# ECS - Elastic Container Service

- It is a service that accepts containers and orchestrates where and how to run those containers
- It is a managed container based compute service
- It runs on two modes: EC2 and Fargate
- Cluster: is the place where container run based on how we want them to run
- Containers are located in container registries (ECR, DockerHub)
- ECS uses **container definitions** to locate images in the container registries, which port should the image use, etc. providing information about the container we want to run
- **Task definition**: represents a self-contained application, can have one or many containers defined in it. A task in ECS defines the application as a whole
- Task definitions store the resources to be used (CPU, memory), networking configuration, compatibility (EC2 mode or Fargate) and also they store the task role (IAM role)
- Task role give permission to ECS containers to access AWS services
- A task does not scale by its own and it is not HA
- ECS service: it is configured by a **service definition**. In a service we define how we want a task to scale, how many copies we like to run
- ECS services define scalability and HA for tasks