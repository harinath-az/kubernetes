# kubernetes


## README: Understanding Kubernetes Solutions for Common Docker Problems

### Introduction

Docker is a powerful platform for containerization, offering lightweight and efficient deployment of applications. However, it faces limitations in managing complex production environments. Kubernetes, an orchestration tool developed by Google, extends Docker's capabilities by providing advanced features for automating the deployment, scaling, and management of containerized applications across clusters of nodes.

### Common Problems with Docker and Kubernetes Solutions

#### Problem 1: Single Host Limitation

**Theoretical Description:** 
Docker containers run on a single host, which poses a risk of resource contention and single point of failure. A memory leak or high resource utilization in one container can impact other containers sharing the same host, potentially leading to service disruptions.

**Scenario:** 
An e-commerce company's website, hosted on Docker containers, experiences a memory leak during a major sale event, causing one container to consume excessive memory. This leads to performance degradation across the entire website due to resource contention on the single host.

**Solution in Kubernetes:**

**Cluster Architecture:** 
Kubernetes operates as a cluster of multiple nodes, each hosting multiple containers (pods). In case of resource issues like a memory leak in one pod, Kubernetes can dynamically schedule pods to other nodes with available resources. This ensures high availability and fault tolerance by distributing workload across the cluster.

**Real-Life Example:** During Black Friday sales, Amazon uses Kubernetes to distribute its services across multiple nodes, ensuring that even if one node encounters issues, other nodes can handle the load seamlessly.

#### Problem 2: Lack of Auto-Healing

**Theoretical Description:** Docker lacks built-in mechanisms for automatic recovery from container failures. When a container crashes due to bugs or resource exhaustion, manual intervention is required to restart the service, resulting in downtime and degraded user experience.

**Scenario:** A streaming service provider hosts multiple services in Docker containers. Occasionally, a container crashes due to a software bug or resource depletion, requiring manual restart to restore service functionality.

**Solution in Kubernetes:**

**ReplicaSets and Deployments:** Kubernetes utilizes **ReplicaSets and Deployments** to manage the lifecycle of pods. If a pod becomes unhealthy or crashes, Kubernetes automatically detects the failure and initiates the creation of a new pod to replace the failed one. This self-healing capability minimizes downtime and ensures consistent service availability.

**Real-Life Example:** Netflix employs Kubernetes to manage its microservices architecture. When a service pod fails, Kubernetes promptly creates a replacement pod, maintaining uninterrupted service delivery during peak usage periods, such as new content releases.

#### Problem 3: Lack of Auto-Scaling

**Theoretical Description:** Docker requires manual intervention for scaling container instances in response to fluctuating application demand. This manual process is time-consuming and prone to human error, particularly during sudden spikes in workload.

**Scenario:** A financial services company experiences a surge in transaction volumes towards the end of the financial year. The Docker-based application struggles to handle the increased load, necessitating manual scaling of container instances to accommodate peak traffic.

**Solution in Kubernetes:**

**Horizontal Pod Autoscaler (HPA):** Kubernetes offers HPA, a feature that automatically adjusts the number of pod replicas based on observed CPU utilization or other custom metrics. When application demand increases, Kubernetes scales out by adding more pod replicas, ensuring optimal performance and resource utilization.

**Real-Life Example:** Stripe utilizes Kubernetes to automatically scale its microservices infrastructure during peak transaction periods. HPA monitors workload metrics and adjusts pod counts accordingly, enabling seamless transaction processing without manual intervention.

#### Problem 4: Lack of Enterprise-Level Features

**Theoretical Description:** Docker lacks native support for enterprise-grade features such as advanced networking, security policies, and compliance controls. Enterprises requiring stringent security measures and regulatory compliance may find Docker's capabilities insufficient.

**Scenario:** A large healthcare organization needs to ensure compliance with strict regulatory requirements for data security and privacy. Docker's basic networking and security features are inadequate to meet these compliance standards.

**Solution in Kubernetes:**

**Ingress Controllers and Network Policies:** Kubernetes provides advanced networking capabilities through Ingress controllers, which manage external access and load balancing for services. Additionally, Kubernetes' Network Policies allow fine-grained control over pod-to-pod communication, enforcing security rules and segmentation within the cluster.

**Real-Life Example:** Cerner utilizes Kubernetes to enhance security and compliance in healthcare applications. Kubernetes' Ingress controllers enable robust traffic management and load balancing, while Network Policies enforce strict access controls and data segregation to meet regulatory requirements.

  
Kubernetes offers robust solutions to real-life problems that companies face when using Docker alone. By addressing single-host limitations, providing auto-healing capabilities, enabling auto-scaling, and offering enterprise-level features, Kubernetes ensures high availability, scalability, and security for containerized applications in production environments. 

For a smooth transition from Docker to Kubernetes, understanding these key features and their practical applications is essential. This knowledge helps in leveraging Kubernetes to its full potential, ensuring efficient and reliable application deployment and management.


# Kubernetes Architecture Overview

Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. Understanding its architecture is crucial for grasping how it manages and orchestrates applications across a cluster of machines.

## Components of Kubernetes Architecture

### Control Plane Components

1. **API Server**
   - **Functionality**: Acts as the gateway for all administrative tasks and user interactions. It validates and processes requests to ensure the cluster's desired state.
   - **Example**: When you deploy a new application using `kubectl`, your command interacts with the API server to initiate the deployment.

2. **etcd**
   - **Functionality**: Key-value store that stores all cluster data persistently. It's used to store configuration details and current state of the cluster.
   - **Example**: Stores information about deployed applications, configurations, and the state of each node in the cluster.

3. **Scheduler**
   - **Functionality**: Assigns pods (groups of containers) to nodes based on resource requirements and constraints.
   - **Example**: When you deploy a pod with specific CPU and memory requirements, the scheduler decides which node in the cluster is best suited to run it.

4. **Controller Manager**
   - **Functionality**: Manages controllers that regulate the state of the cluster and ensure the desired state is maintained.
   - **Example**: The ReplicaSet controller ensures a specified number of pod replicas are running, managing scaling and fault tolerance automatically.

5. **Cloud Controller Manager**
   - **Functionality**: Interfaces with cloud provider APIs to manage cloud-specific resources like load balancers and storage volumes.
   - **Example**: When you create a LoadBalancer service in Kubernetes, the Cloud Controller Manager interacts with the cloud provider's API to provision the load balancer.

### Data Plane Components (Worker Nodes)

1. **kubelet**
   - **Functionality**: Acts as an agent on each node, responsible for managing pods and ensuring they maintain their desired state.
   - **Example**: Monitors the health of pods and restarts them if they fail to maintain availability.

2. **kube-proxy**
   - **Functionality**: Manages network connectivity for pods, including routing, load balancing, and firewall rules.
   - **Example**: Routes incoming traffic to the correct pods based on service definitions and manages cluster networking using `iptables` rules.

3. **Container Runtime**
   - **Functionality**: Executes container images within pods on nodes, providing the environment needed for applications to run.
   - **Example**: Runs Docker containers or other container runtimes specified in the pod's configuration, ensuring applications run reliably.

### Comparing with Docker

- **Container Runtime**: In Docker, the Docker Engine provides container runtime capabilities similar to Kubernetes' Container Runtime. Both execute container images but Kubernetes supports multiple runtimes like Docker, containerd, and CRI-O.
  
- **Networking**: Docker uses a default bridge networking, whereas Kubernetes uses `kube-proxy` for networking. Both manage network traffic but Kubernetes adds advanced networking features like service discovery and load balancing.

- **Orchestration**: While Docker provides basic container management, Kubernetes goes further with a robust control plane (API Server, etcd, Scheduler, Controller Manager) that automates scaling, self-healing, and service discovery across clusters.

---

By understanding Kubernetes architecture and its components, you gain insights into how it manages containerized applications effectively in cloud-native environments compared to Docker's simpler container management capabilities. This knowledge is essential for deploying and maintaining applications at scale with Kubernetes.




# Managing Kubernetes Clusters with KOPS

## Overview
Understanding how DevOps Engineers manage the lifecycle of Kubernetes clusters in production, including creation, upgradation, configuration, and deletion is important because many people practice Kubernetes using Minikube or other local setups like Minikube, K3s, k3d, or MicroK8s. While these platforms are excellent for learning and exploring Kubernetes, they are all development environments and not suitable for production use. Even the official Minikube documentation states that it is a local Kubernetes cluster and should not be used in production.For DevOps positions, one of your primary responsibilities would be to create infrastructure for your organization, including managing the lifecycle of Kubernetes clusters. In production, you won't be using Minikube, k3s; instead, you will use Kubernetes distributions.

A Kubernetes distribution is a customized version of the Kubernetes platform. For example, Amazon has EKS, Red Hat has OpenShift, VMware has Tanzu, and Rancher Labs has Rancher. These distributions provide additional features, support, and timely updates, ensuring better customer experience and production readiness. 

In interviews, you will be asked about the Kubernetes distributions you have used in production and how you managed their installation and upgrades. Knowing popular distributions like Kubernetes, OpenShift, Rancher, VMware Tanzu, EKS, AKS, and GKE is essential.Local Kubernetes clusters are for development, while production environments use distributions to ensure better support and management.

### Docker Swarm vs. Kubernetes

Docker Swarm is a standalone container orchestration engine, while Kubernetes distributions like Minikube and MicroK8s are tailored for development and testing purposes. Deploying Kubernetes directly harnesses its full enterprise capabilities, including robust storage management and scalability features.

### Kubernetes vs. Amazon EKS

Deploying Kubernetes on EC2 instances grants full control but lacks support from AWS for Kubernetes-related issues. In contrast, Amazon EKS provides managed Kubernetes clusters with AWS support, including additional tools like EKSCTL for streamlined management.

## Using KOPS for Kubernetes Operations

KOPS simplifies the lifecycle management of Kubernetes clusters, handling everything from initial deployment to upgrades and deletions. Unlike manual setups or other tools like Cube EDM, KOPS automates these processes, making it ideal for DevOps engineers managing multiple production clusters.

# Kubernetes Installation Using KOPS on EC2

## Prerequisites

1. An EC2 instance or your personal laptop.
2. Dependencies:
    - Python 3
    - AWS CLI
    - kubectl

## Install Dependencies

1. Add the Kubernetes apt repository:

    ```bash
    curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
    echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
    sudo apt-get update
    ```

2. Install required packages:

    ```bash
    sudo apt-get install -y python3-pip apt-transport-https kubectl
    pip3 install awscli --upgrade
    export PATH="$PATH:/home/ubuntu/.local/bin/"
    ```

## Install KOPS

1. Download and install KOPS:

    ```bash
    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
    chmod +x kops-linux-amd64
    sudo mv kops-linux-amd64 /usr/local/bin/kops
    ```

## Provide the below permissions to your IAM user. If you are using the admin user, the below permissions are available by default

Ensure your IAM user has the following permissions:
- AmazonEC2FullAccess
- AmazonS3FullAccess
- IAMFullAccess
- AmazonVPCFullAccess


## AWS CLI Configuration

Set up AWS CLI configuration on your EC2 instance or laptop:

```bash
aws configure
```

## Kubernetes Cluster Installation

Please follow the steps carefully and read each command before executing.

1. Create an S3 bucket for storing KOPS objects:

    ```bash
    aws s3api create-bucket --bucket kops-abhi-storage --region us-east-1
    ```

2. Create the Kubernetes cluster:

    ```bash
    kops create cluster --name=demok8scluster.k8s.local --state=s3://kops-abhi-storage --zones=us-east-1a --node-count=1 --node-size=t2.micro --master-size=t2.micro --master-volume-size=8 --node-volume-size=8
    ```

**3. Important: Edit the configuration as there are multiple resources created which won't fall into the free tier.**

    ```bash
    kops edit cluster demok8scluster.k8s.local
    ```

4. Build the cluster:

    ```bash
    kops update cluster demok8scluster.k8s.local --yes --state=s3://kops-abhi-storage
    ```

    This will take a few minutes to complete.

5. Verify the cluster installation:

    ```bash
    kops validate cluster demok8scluster.k8s.local
    ```

### Cost Optimization Strategies for Managing Multiple Kubernetes Clusters

1. **Use of KOPS (Kubernetes Operations)**: By using kOPS for cluster management, engineers can efficiently handle the lifecycle of Kubernetes clusters, including upgrades, modifications, and deletions. This tool streamlines operations and reduces manual intervention, leading to cost savings.

2. **Use of Local Domains for Non-Production Environments**: For development and staging environments, engineers use local domains (e.g., .k8s.local) instead of purchasing and configuring custom domains. This avoids the cost of domain purchases and Route 53 configurations.

3. **Minimizing Resource Allocation**: When creating clusters, engineers carefully configure the number and size of nodes, EBS volumes, and other resources to match the actual requirements, avoiding over-provisioning and unnecessary costs.

4. **Using Minikube for Local Testing**: For local development and testing, engineers use Minikube instead of full-fledged Kubernetes clusters on AWS. This lightweight alternative runs Kubernetes clusters locally on their machines, eliminating cloud resource costs.


