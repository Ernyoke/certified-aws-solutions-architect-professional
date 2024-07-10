# EKS - Elastic Kubernetes Service

## Kubernetes 101

- It is an open source container orchestration system
- We used to automate the deployment, scaling and management of containerized applications
- It is a cloud-agnostic product, can be used on-premises as well
- Cluster Structure:
    - A Kubernetes cluster is a HA cluster of compute resources organized to work as one unit
    - Cluster starts with the Cluster Control Plane: manages the cluster, scheduling, applications, scaling and deploying
    - Compute in Kubernetes is provided via Cluster Nodes: VM or physical servers which function as a worker in the cluster. These run the containerized applications
    - Software that will run on each node:
        - On each of the nodes runs a container runtime: `containerd` or other software used to handle container operations
        - `kubelet`: the agent which interacts with the control plane is. This uses the Kubernetes API to communicate with the control plane
- Cluster details:
    - Pods: 
        - The smallest units of computing in Kubernetes
        - Pods can have multiple containers and provide shared storage and networking for them
        - It is common to have one container per pod, although we can have multiple container in a pod
        - Pods are not permanent: they can be deleted when finished with a job, evicted because of lack of resources or when a node fails
    - Control plane runs the following software:
        - `kube-apiserver`: 
            - The front-end for the Kubernetes control 
            - It is what nodes and other cluster elements interact with
            - Can be horizontally scaled for HA and performance
        - `etcd`:
            - HA key/value store
            - Backing store for the cluster
        - `kube-scheduler`:
            - Identifies any pods within a cluster that does not have a node assigned
            - Assigned pods based on resources, deadlines, affinity, data locality and any other constraints
        - `cloud-controller-manager`:
            - Optional component
            - Provides cloud-specific control logic
            - It allows us to link Kubernetes with cloud providers APIs such as AWS
        - `kube-controller-manager`:
            - It is a collection of processes:
                - Node Controller: monitoring and responding to node outages
                - Job Controller: one-of tasks (jobs)
                - Endpoint Controller: populates endpoints 
                - Service Account and Token Controllers: accounts/API tokens
    - kube-proxy:
        - Every node runs it
        - It is a networking proxy
        - It coordinates networking with the control plane
        - It helps implement "services" and configures rules allowing communications with pods from inside or outside of the cluster
- Ingress: exposes a way into a service from outside to the cluster
- Ingress Controller: a piece of software with arranges the underlying hardware to allow ingres (example: AWS LB Controller which use ALB/NLB)
- Persistent Storage (PV): they are volumes whose lifecycle lives beyond any one single pod that is using it. "Normal" long running storage

## Elastic Kubernetes Service 101

- It is an AWS managed implementation of Kubernetes as a service
- EKS can be run in different ways:
    - On AWS itself as a product
    - On Outposts: racks and servers operating in on-premises locations, but controlled and managed by AWS
    - EKS Anywhere: EKS clusters running on on-premises or anywhere else
    - EKS Distro: EKS product as open-source
- The EKS control plane is managed by AWS and scales based on load. It runs across multiple AZs
- Integrates with other AWS services such as ECR, ELB, IAM, VPC
- EKS Cluster = EKS Control Plane and EKS Notes
- etcd is also managed by AWS and distributed across multiple AZs
- Nodes can be the following types:
    - Self managed: EC2 instances managed by us
    - Managed node groups: still EC2 instances, but EKS manages the provisioning and lifecycle managing
    - Fargate: nodes are managed by EKS, we don't have to worry about scaling and optimizing clusters (similar to ECS Fargate)
- Choosing between node types depends on for what we want to use the cluster. If we want Windows nodes, GPU, Inferentia, Bottlerocket, Outposts, Local zones, etc. we need to check which types of nodes support the feature we require
- Persistent storage on EKS: it can use EBS, EFS, FSx for Lustre, FSx for NetApp ONTAP as storage providers