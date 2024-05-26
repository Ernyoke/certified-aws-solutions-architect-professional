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

## ECS Cluster Modes

- Cluster mode define how much of the admin overhead is required for running containers in ECS (what parts do we manage and what parts does AWS manage)
- Cluster modes are:
    - EC2 Mode
    - Fargate Mode

### EC2 Mode

- Uses EC2 instances which are running inside of a VPC
- Since we are inside of a VPC, we can benefit from using multiple AZs
- When we create the cluster, we specify the initial size of containers
- Horizontal scaling for EC2 instances and for ECS tasks is controlled by ASGs
- With EC2 cluster mode we are paying for EC2 instances independently of what containers and how many of containers are running on them

### Fargate Mode

- We don't have to manage EC2 instances for use as container host
- With Fargate there are no servers to manage
- AWS maintains a shared Farge infrastructure platform offered to all users
- We gain access to resources from a shared pool, we don't have visibility for other customers
- A Fargate deployment still uses a cluster with a VPC which operates in AZs
- ECS tasks are injected into the VPC with an ENI, they are running on the Fargate shared platform
- With Fargate mode we only pay for the containers we using based on the resources they consume

## EC2 vs ECS (EC2) vs Fargate

- If we are already using containers, we should use ECS
- Containers make sense if we want to isolate applications
- We generally pick EC2 mode if we have a large workload and the business is price conscious
- Historically EC2 mode was giving the most value for the price if we were using saving plans. Nowadays we can have savings plan for Fargate and Lambda, so we should default ot Fargate instead of EC2 mode
- If we are overhead conscious, we should use Fargate
- For small/burst workloads we should use Fargate as well. Same is recommended for batch/periodic workloads