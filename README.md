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


# Kubernetes Application Deployment Guide

## Key Advantages of Kubernetes
- **Cluster Management**
- **Auto-scaling**
- **Auto-healing**
- **Enterprise-level Features**

## Essential Terminologies
Understanding these terminologies is crucial before deployment:
- **Pod:** The smallest deployable unit in Kubernetes, which can contain one or more containers.

## Deployment Process

### Pod Definition
In Kubernetes, the lowest level of deployment is a pod. Unlike Docker, where you directly build and deploy containers, Kubernetes uses pods to manage containers. A pod is essentially a description of how to run a container. For instance, in Docker, you would use command-line arguments to run a container, specifying details such as the container name, image, port mapping, volume mounts, and network settings. In Kubernetes, these specifications are defined in a YAML file, making it easier to manage and deploy applications.

<img src="https://github.com/harinath-az/kubernetes/blob/main/images/pods.png" width="650" height="300">

A pod is a concept similar to a container, but it abstracts user-defined commands in a pod specification YAML file. Instead of deploying a container directly, you deploy a pod, which can contain one or multiple containers. A single-container pod is essentially like a Docker container, but instead of using the docker run command with various arguments, you specify everything in a YAML file. This file includes the API version (e.g., v1), the name of the pod, and the specifications of the container(s) within the pod. This approach provides a declarative way to manage applications, which is a key feature of Kubernetes as an enterprise-level platform. Kubernetes uses YAML files for everything, including pod resources, deployments, and services, enabling standardization and declarative capabilities.It's essential to understand how to write and interpret these YAML files, as they are central to managing Kubernetes resources. While there are many examples available from official documentation and other sources, becoming proficient with YAML files is crucial for working effectively with Kubernetes.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-first-pod
spec:
  containers:
  - name: my-container
    image: my-image:latest
    ports:
    - containerPort: 80
```

## Advantages
In Kubernetes, a pod can contain one or more containers, offering flexibility in deployment scenarios. While most pods typically host a single container, there are cases where multiple containers are used within the same pod. This setup is beneficial for scenarios involving sidecar containers or init containers, which provide supplementary functions to the main container. For example, if an application container needs to access configuration files or user data from another container, it is more efficient to place both containers within the same pod rather than creating separate pods. By doing so, Kubernetes ensures that containers within the same pod can **share networking and storage**. This means that containers can communicate using localhost and share files seamlessly. 

The main advantage of grouping containers within a single pod is simplified management and interaction. Containers within the same pod can share the same IP address, allocated by Kubernetes, allowing access via this **cluster IP address** rather than individual container IPs. This pod abstraction helps DevOps engineers manage complex deployments more easily. Instead of dealing with individual containers directly, they can use YAML files to define pod specifications, providing a clear and standardized approach to container management. This abstraction is particularly useful in large-scale environments with numerous containers, making it easier to handle and understand container configurations and interactions.

# kubectl

Just like how we need Docker CLI in order to interact with Docker, `kubectl` is the command-line interface (CLI) tool used to interact with a Kubernetes cluster. It provides a way to manage and control Kubernetes resources, such as pods, deployments, and services. With `kubectl`, users can perform a variety of operations including deploying applications, scaling deployments, rolling out updates, and inspecting cluster resources. The tool communicates with the Kubernetes API server to execute commands and retrieve information, enabling users to manage cluster configurations and troubleshoot issues effectively. Through simple and powerful commands, `kubectl` is essential for both everyday operations and advanced cluster management in Kubernetes.

# Minikube and Kubernetes Cluster Setup

## Introduction

Minikube is a lightweight Kubernetes implementation that creates a virtual machine on your local machine to run a single-node Kubernetes cluster. This setup is ideal for development and testing purposes.

## Installing Minikube

To get started with Minikube, you need to install both Minikube and `kubectl`. If Minikube is already installed on your machine, you can proceed directly to setting up your Kubernetes cluster. To verify the installation, you can run `minikube version`.

## Starting Minikube

Once Minikube is installed, create a Kubernetes cluster using the command:

```bash
minikube start
```

On MacOS or Windows, Minikube creates a virtual machine to host a single-node Kubernetes cluster. This setup uses a virtualization platform like Hyperkit or VirtualBox. By default, Minikube may use Docker as the driver, but you can specify a different driver with the `--driver` flag, e.g., `--driver=hyperkit` or `--driver=virtualbox`.

## Verifying Cluster Setup

After starting Minikube, you can check if your Kubernetes cluster is up and running by using:

```bash
kubectl get nodes
```

You will see one node, named `minikube`, which serves as both the control plane and data plane due to the single-node architecture. 

## Creating and Managing Pods

To deploy a pod, you need to create a YAML file that defines the pod configuration. For instance, using the default Nginx image:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

Save this YAML configuration to a file (e.g., `pod.yaml`) and create the pod with:

```bash
kubectl create -f pod.yaml
```

To check the status of the pod, use:

```bash
kubectl get pods
```

For detailed information, including IP address and status, use:

```bash
kubectl get pods -o wide
```

## Accessing the Pod

To access the pod and execute commands, use:

```bash
minikube ssh
```

Alternatively, for debugging, you can use:

```bash
kubectl logs [pod-name]
kubectl describe pod [pod-name]
```

These commands help you view logs and get detailed information about the pod's state, which is useful for troubleshooting.

## Cleaning Up

To delete the pod, use:

```bash
kubectl delete pod [pod-name]
```

## Next Steps

As you progress, you'll learn about advanced Kubernetes features such as deployments, scaling, and high availability. The pod is the fundamental unit of deployment in Kubernetes, but for production scenarios, you will typically use deployments, which offer additional features like auto-scaling and rolling updates.

## Additional Resources

For a comprehensive list of `kubectl` commands, refer to the [Kubernetes Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/). This resource is invaluable for understanding and mastering Kubernetes commands.


# Understanding Kubernetes Deployment
## Kubernetes Deployment
To understand Kubernetes deployment, it's important to have understanding about Kubernetes pods. Let’s explore the differences between a container, a pod, and a deployment, which is also a common interview question.

### Containers
Containers can be created using various container platforms like Docker. To run a container, you provide specifications through the command line, such as:

- `docker run -it` or `-d` for detached mode
- The name of the image
- Port exposure using `-p`
- Volume mapping using `-v`
- Network settings using `--network`

### Pods
Kubernetes modifies this process by allowing you to create a YAML manifest that defines all these specifications instead of writing them in the command line. A pod YAML manifest includes details like the container image, port, volumes, and network settings. A pod can contain one or multiple containers, which can share networking and storage, making it suitable for applications that depend on other applications.

### Deployments
While a pod can deploy an application, it lacks capabilities like auto-healing and auto-scaling. Deployments in Kubernetes offer these features. A deployment manages pods and ensures they run efficiently. It can automatically heal and scale applications, ensuring high availability and performance.

### Key Differences
- **Container:** Runs individual applications using Docker commands.
- **Pod:** Groups one or more containers, sharing the same network and storage, defined using a YAML manifest.
- **Deployment:** Manages pods, providing auto-healing and auto-scaling capabilities, ensuring zero downtime and efficient resource management.

## Why Use Deployments?
Deployments are essential for maintaining the desired state of applications in Kubernetes. They allow you to:
- Perform rolling updates and rollbacks
- Scale applications seamlessly
- Ensure high availability with minimal manual intervention

# Kubernetes Deployment Resource

## Introduction

In Kubernetes, it is recommended not to create pods directly but to use a **deployment resource** instead. This resource is crucial for managing pods efficiently and ensuring high availability and load balancing.

## How It Works

A **deployment resource** first creates a **replica set**, which is a Kubernetes controller, and then rolls out the pods. This intermediate resource allows you to specify the number of pod replicas required. For instance, you might need to handle high loads by directing users to different pod replicas.You can configure your deployment YAML manifest to specify that 100 users should go to replica 1 of pod and another 100 to replica 2 of the pod, effectively distributing the load. In Kubernetes, everything is defined using YAML manifests. When you specify a replica count in the deployment YAML, the deployment will use a replica set to create the required number of pods. The replica set ensures that the specified number of pods is always running, implementing auto-healing and auto-scaling. If a user accidentally deletes a pod, the replica set will recreate it to maintain the desired number of replicas. Additionally, if you update the replica count in the YAML manifest, the replica set will adjust the number of pods accordingly.

This zero-downtime deployment feature ensures that applications remain available and performant. In Kubernetes, controllers like the replica set maintain the desired state by ensuring that the actual state on the cluster matches the specifications in the YAML manifest. There are default controllers provided by Kubernetes, and custom controllers like ArgoCD can also be created. Understanding these concepts is crucial for managing Kubernetes deployments effectively.

<img src="https://github.com/harinath-az/kubernetes/blob/main/images/deployment.png" width="650" height="300">

## Auto-Healing and Auto-Scaling

If a pod is accidentally deleted, the replica set recreates it to maintain the desired number of replicas. Updating the replica count in the YAML manifest adjusts the number of pods accordingly, ensuring zero-downtime deployment. The replica set, being a Kubernetes controller, maintains the desired state by ensuring the actual state on the cluster matches the YAML manifest specifications.

## Controllers in Kubernetes

Controllers in Kubernetes manage the state, ensuring the desired and actual states are consistent. There are default and custom controllers, like ArgoCD. The **replica set controller** ensures the desired number of pod replicas are always running, maintaining application availability and performance.

# Interacting with Kubernetes using kubectl and implementation of Deployment

## Basic Commands
## Starting Minikube

Once Minikube is installed, create a Kubernetes cluster using the command:

```bash
minikube start
```

On MacOS or Windows, Minikube creates a virtual machine to host a single-node Kubernetes cluster. This setup uses a virtualization platform like Hyperkit or VirtualBox. By default, Minikube may use Docker as the driver, but you can specify a different driver with the `--driver` flag, e.g., `--driver=hyperkit` or `--driver=virtualbox`.

Once we have started the minikube, we can create the pod with the help of yaml manifest file by using `kubectl create -f [pod-name]`
### Viewing Resources

To view all the resources in your cluster, use:
```sh
kubectl get all
```
This command lists all resources, including pods, deployments, and services in the current namespace. To list resources across all namespaces, use:
```sh
kubectl get all -A
```

### Deleting Resources

To delete a deployment, use:
```sh
kubectl delete deploy <deployment_name>
```
After deletion, you can verify by running:
```sh
kubectl get pods
kubectl get deploy
```

## Creating and Managing Pods

### Creating a Pod

To create a pod, use a YAML manifest. For example, with `pod.yaml`:
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx:latest
```
Apply this manifest with:
```sh
kubectl apply -f pod.yaml
```
Verify the pod creation with:
```sh
kubectl get pods
```

### Viewing Pod Details

To view detailed information about the pod:
```sh
kubectl get pods -o wide
```
For more details:
```sh
kubectl describe pod <pod_name>
```

### Accessing the Pod

If you're using Minikube, you can SSH into the cluster:
```sh
minikube ssh
```
Then, use `curl` to access the application:
```sh
curl <pod_ip_address>
```

## Importance of Deployments

### Why Use Deployments?

Deployments manage your pods and ensure high availability. If a pod is accidentally deleted or fails, the deployment controller will automatically recreate it. This auto-healing feature ensures your application remains available.

### Creating a Deployment

Here's an example `deployment.yaml`:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
```
Apply this deployment with:
```sh
kubectl apply -f deployment.yaml
```

### Managing Deployments

To see the deployment and the created pods:
```sh
kubectl get deploy
kubectl get pods
```
To view the replica set:
```sh
kubectl get rs
```

### Updating Deployments

To scale the deployment, update the `replicas` field in the `deployment.yaml` and reapply:
```sh
kubectl apply -f deployment.yaml
```
Verify the changes:
```sh
kubectl get pods
```

### Auto-Healing Demonstration

To see auto-healing in action, watch the pods while deleting one:
```sh
kubectl get pods -w
```
In another terminal, delete a pod:
```sh
kubectl delete pod <pod_name>
```
You will see the pod being terminated and a new one being created immediately.
<img src="https://github.com/harinath-az/kubernetes/blob/main/images/watch.png" width="650" height="300">

## Deleting the depoyment created
To stop or delete a Kubernetes deployment that was created using `kubectl create -f deployment.yaml`, you can use the `kubectl delete` command. Here are the steps:

1. **Identify the Deployment Name:**
   Ensure you know the name of the deployment. If you are unsure, you can list all deployments in the namespace:
   ```bash
   kubectl get deployments
   ```

2. **Delete the Deployment:**
   Use the `kubectl delete` command with the name of the deployment:
   ```bash
   kubectl delete deployment <deployment-name>
   ```
   Replace `<deployment-name>` with the actual name of your deployment.

3. **Delete Using the YAML File:**
   Alternatively, you can delete the deployment using the same YAML file you used to create it:
   ```bash
   kubectl delete -f deployment.yaml
   ```
### Verifying Deletion:

After deleting the deployment, you can verify that it has been removed by listing the deployments again:
```bash
kubectl get deployments
```

This should show that the deployment is no longer present.


# Kubernetes Services 

In production scenarios, we don't deploy a pod directly; instead, we usually deploy a deployment. Deployments are crucial because they help manage the lifecycle of pods, ensuring that the desired number of replicas is maintained. **Then why do we need services?**

### Scenario Without Services

Assume Kubernetes didn't have the concept of services. A developer or DevOps engineer would deploy their pod as a deployment in Kubernetes. The deployment creates a replica set, which in turn creates pods. For example, if we need three replicas, the replica set creates three pods.Why do we need multiple replicas? If only one user is accessing an application, one pod suffices. However, if there are multiple concurrent users (e.g., many users accessing WhatsApp simultaneously), a single pod can get overwhelmed. Therefore, we create multiple replicas to handle the load, ensuring the application can handle concurrent requests.

#### Problem Without Services

Let's consider a scenario where one of the pods goes down. Kubernetes has an auto-healing capability, where the replica set recreates the pod. However, when the pod comes back up, its IP address changes. Previously, the pod might have had an IP like 172.16.3.4, but after coming back up, it might be 172.16.3.8. This change means that any team or application trying to access the pod using the old IP address will fail.In real-world applications, like Google, users never access the application using specific IP addresses. Instead, they use a domain name (e.g., google.com), and load balancing is handled behind the scenes.

1. **Pods Going Down**: In Kubernetes, containers (pods) are ephemeral and may go down frequently. Kubernetes’ auto-healing feature ensures that when a pod goes down, a new one is created by the replica set. However, the new pod gets a different IP address.
   
2. **IP Address Change**: When the new pod comes up, it might have a different IP address than the previous one. For instance, if the original IP was `172.16.3.4`, the new IP might be `172.16.3.8`.

3. **Communication Issues**: This IP change creates a communication problem. If other applications or users were accessing the old IP, they would lose connectivity. Sharing IP addresses with users or other teams becomes impractical as they keep changing.


## Introduction to Services in Kubernetes

To address the above issues, Kubernetes uses services. A service is an abstraction that defines a logical set of pods and a policy to access them. Services enable a stable endpoint for accessing a group of pods.In Kubernetes, instead of accessing pods directly using their IP addresses, we create a service. This service acts as a **load balancer.** For example, if the service name is payment, users access the application via payment.default.svc.The service forwards incoming requests to the appropriate pods, distributing the load among them. This way, even if a pod's IP address changes, the service ensures that the application remains accessible.

#### Key Advantages of Kubernetes Services
1. **Load Balancing**: Distributes incoming traffic across multiple pods.
2. **Service Discovery**: Allows internal services to find each other via DNS.
3. **External Exposure**: Exposes applications to the external world, enabling access from outside the Kubernetes cluster.

**Service Discovery**

You might wonder, if a pod's IP address changes, won't the service also face the same issue? This is where Kubernetes' **service discovery** mechanism comes in. Instead of keeping track of IP addresses, the service uses **labels and selectors**. Each pod is assigned a label (e.g., `app=payment`). The service keeps track of pods using these labels. So, even if a pod's IP address changes, as long as the label remains the same, the service can route traffic to the correct pod.

**Labels and Selectors**

When creating a deployment, you specify a label in the metadata. For example:

```yaml
metadata:
  labels:
    app: payment
```

This label is applied to all pods created by the deployment. If a pod goes down and comes back up with a new IP address, the label remains the same. The service uses this label to discover and route traffic to the correct pods.

### Features of Services

- **Load Balancing**: Services provide load balancing across the pods. For example, if there are three pods, the service will distribute the traffic among them.
  
- **Stable Endpoints**: Instead of accessing individual pod IPs, users or other applications access the service, which internally routes the traffic to the appropriate pods.

### Example

Assume you have a deployment with three pods having IPs `172.16.3.4`, `172.16.3.5`, and `172.16.3.6`. You create a service that abstracts these pods. Users or applications interact with the service instead of the individual pod IPs.

- **Users/Applications**: Instead of accessing `172.16.3.4`, `172.16.3.5`, or `172.16.3.6`, they access the service endpoint.
- **Service**: The service routes the request to one of the pods based on the load balancing policy.

### Kubernetes Service: Exposing Applications to the External World
One of the significant features of Kubernetes services is their ability to expose applications to the external world. Apart from service discovery and load balancing, services in Kubernetes can make your applications accessible to external users.

In Kubernetes, whenever you create a deployment, the pods receive an internal IP address, such as 172.16.3.4. While this internal IP allows for communication within the cluster, it doesn't suffice for external access. For instance, users cannot be expected to SSH into the Kubernetes cluster to access applications. Instead, they should be able to access the application via a simple URL, similar to how you access Google at `https://google.com` without needing SSH.

Kubernetes services provide three primary types for exposing applications: ClusterIP, NodePort, and LoadBalancer.

1. **ClusterIP**: This is the default service type in Kubernetes. It exposes the service on an internal IP within the cluster, making it accessible only from within the cluster. This type is ideal for internal applications where external access is not required. For example, if you configure a service with a ClusterIP, only components within the Kubernetes cluster can access the service, benefiting from the load balancing and service discovery features provided by Kubernetes.

2. **NodePort**: This service type exposes the service on a static port on each node's IP address. It allows access to the service from outside the cluster but only within the organization’s network. Users who have access to the node’s IP addresses can reach the service. This type is useful for scenarios where the application needs to be accessible within a secure network but not directly on the internet.

3. **LoadBalancer**: This service type is used to expose the service to the internet. It provisions a load balancer provided by the cloud provider (like AWS’s Elastic Load Balancer) and assigns a public IP address. This type is suitable for applications that need to be accessible globally. When you create a LoadBalancer service, Kubernetes communicates with the cloud provider to set up the external load balancer, which forwards traffic to the service.
<img src="">

Understanding the different service types is crucial for configuring how your applications are accessed. If you need the application to be globally accessible, the LoadBalancer service type is appropriate. For internal applications, ClusterIP or NodePort can be used based on the required access scope.

### Example
When you create a deployment in Kubernetes, the pods receive an internal IP address like 172.16.3.4. This IP address is only accessible within the cluster. To expose the application to the external world, you need to create a Kubernetes service. When you create a service of type **ClusterIP**, the service will only be accessible within the Kubernetes cluster. If you create a service of type **NodePort**, the service will be accessible within your organization’s network, allowing users who have access to the node’s IP addresses to reach the service. If you create a service of type **LoadBalancer**, the service will be accessible to the internet, with a public IP address assigned by the cloud provider. This public IP address allows users from anywhere in the world to access the application.


# Kubernetes Ingress and Ingress Controller

## Introduction

Before 2015, there was no concept of Ingress, people were using Kubernetes services and that is when they encountered problems, the two main problems Faced by them were **Lack of Enterprise &TLS based Load balancing and Load balancer mode problem** . The customers kept on complaining on Kubernetes GitHub page that when we were on Virtual machines we were enjoying all the good capabilities of load balancers. Okay and because of which our applications were very secure because of which you know we had reduced cost but once we moved to Kubernetes we realized that this is a very big problem. So Kubernetes people have also agreed to it and what people at Kubernetes said is okay we will implement something called **Ingress**.

## Virtual Machines vs. Kubernetes
The people when they used to use VMs or physical servers employed by enterprise load balancers like Nginx, F5, and others, offering advanced features such as:

**Ratio-based load balancing
Sticky sessions
Path-based load balancing
Host/domain-based load balancing
Whitelisting and blacklisting
TLS-based load balancing (secure HTTPS)**

But the kubernetes used to provide simple load balancing, as they were getting more features with the Legacy approach, they were not happy and the other reason being the cloud provider used to charge for each static IP that was created and Huge applications will have 1000's of services, these services when using Load balancer mode would cost the organizations so much more money when compared to the Virtual Machines, where people used to create a Load balancer irrespective of the number of applications, they used to configure load balancer telling, if the request is coming to amazon.com/abc then send request to app1 and if it is coming to amazon.com/xyz send it to app2 and They used to expose this single load balancer using Static IP.

So kubernetes came up with a solution of creating Ingress, saying we will allow the users of Kubernetes to create a resource called Ingress and since it is not practically possible to do the implementatio for each and every loadbalancer kind, kubernetes told the loadbalancer providers like NGINX, F5 to create something called as **Ingress controller**. Kubernetes told it will tell its users to create something called as an Ingress resource.Now what is this Ingress controller? Okay so on a high level if you are creating Ingress resource on your Kubernetes cluster and if you are saying that I need a path-based routing for example. Okay so you realize that you are missing the path-based routing on Kubernetes which you are very heavily using on your Virtual machines. So you can come to your Kubernetes cluster create an Ingress resource. I want to create path-based routing so you can use Kubernetes to create a YAML file and inside the YAML file say that you know I want path-based routing. So you said the same thing but who will implement this? Okay so who will decide that which load balancer you want to use. So there are hundreds of load balancers in the market so what Kubernetes said is okay we cannot support all of you, you know we cannot create the logic for all of you in the Kubernetes master or the API server. Instead kubernetes asked the load balancer providers like Nginx, F5 people to create something called as Ingress controller. So let's say that you want to create this specific capability using NGINX load balancer so the NGINX company will write a NGINX Ingress controller and as Kubernetes users on this Kubernetes cluster you will deploy the Ingress controller. Okay you can deploy that using Helm charts you can deploy that using YAML manifest. Once you deploy the developer or again the DevOps engineers they will create the Ingress YAML service YAML resource for their Kubernetes services. Okay so this Ingress controller will watch for the Ingress resource and it will provide you the path-based routing.

For example let's say you are creating a pod in your kubernetes cluster.you are writing a YAML manifest for this and you have created a pod now what will happen like I told you there is a component called kubelet. This kubelet will deploy your pod onto one of the worker nodes so kubelet will also sit on the worker node and API server will notify kubelet using scheduler that okay a pod is created and kubelet will deploy the pod.Right and similarly let's say you are creating a service YAML manifest. Okay so there is kube-proxy and this kube-proxy will what it will do this kube-proxy will update the IP table rules so for every resource that you are creating in Kubernetes there is a component which is watching for that resource and it is performing the required action. Okay so similarly even if you are creating Ingress in Kubernetes let's say you are creating Ingress.**There has to be a resource or component or a controller which has to watch for this Ingress. Right so this was the problem so Kubernetes said that okay I can create Ingress resource but if I have to implement logic for all the load balancers that are available in the market that is NGINX, F5, Traefik, Ambassador, HAProxy.Kubernetes said that okay it is technically impossible I cannot do it so what I'll do is I'll come up with the architecture and the architecture is user will create Ingress resource.and load balancing companies like NGINX, F5 or any other load balancing companies they will write their own Ingress controllers and they will place these Ingress controllers on GitHub and they will provide the steps on how to install these Ingress controllers using Helm charts or any other ways and as a user instead of just creating Ingress resource you also have to deploy Ingress controller. Okay so it is up to the organization to choose which Ingress controller they want to use what is Ingress control at the end of the day it is just a load balancer. Right sometimes it can be a load balancer plus API Gateway as well API Gateway offers you some additional capabilities.**
<img src="">

Okay so end of the day what you need to do as a user is on your Kubernetes cluster the prerequisite is deploy an Ingress controller. Which Ingress controller you will deploy let's say in your Virtual machines world before you move to Kubernetes if you are using NGINX so you will go to NGINX GitHub page and you will deploy the NGINX Ingress controller onto the Kubernetes cluster. After that you will create Ingress resource depending upon the capabilities that you need. Okay if you need path-based routing you will create one type of Ingress, if you need TLS-based Ingress you will create one type of Ingress, if you need host-based you will create one type of Ingress so this is one-time activity. The one-time activity for the DevOps engineers is to decide which Ingress controller they want, what is Ingress controller to decide which load balancer they want. Okay it can be NGINX it can be F5 and they will go to their organizational GitHub page they will find the steps on how to deploy this and once they realize how to deploy they will after that it can be one service, two service, 100 services they will only write the Ingress resource once they write the Ingress resource. Like you know Ingress does not have to be one-to-one mapping. Okay you can create one Ingress and you know you can handle 100 services as well using paths. You can say if path is A go to service one, if path is B go to service two I'll show you that don't worry about it but you understood the topic here right what was the problem why Ingress was introduced what is Ingress controller you understood all of these things so once you understand this concept it is very easy for you.

### Ingress Resource and Ingress Controller

#### Ingress Resource
**Definition:** An Ingress resource is a set of rules that define how external HTTP/HTTPS traffic should be routed to services within your Kubernetes cluster.
Content: It includes specifications like:
Host-based routing: Directing traffic based on the requested hostname.
Path-based routing: Directing traffic based on URL paths.
TLS/SSL: Handling secure connections.

# Ingress Controller 

## Definition
An **Ingress Controller** is a specialized controller in Kubernetes that implements the rules defined in Ingress resources.

## Function
The Ingress Controller watches for changes to Ingress resources and configures the underlying proxy/load balancer to enforce the rules.

## How They Work Together

### 1. Create an Ingress Resource
- You define an Ingress resource with the desired routing rules.
- This resource is submitted to the Kubernetes API.

### 2. Ingress Controller Watches Ingress Resources
- The Ingress Controller continuously monitors the Kubernetes API for any new or updated Ingress resources.

### 3. Ingress Controller Configures Proxy
- When it detects a new or updated Ingress resource, the Ingress Controller translates the rules into configurations for the underlying proxy (e.g., NGINX, HAProxy, Traefik).
- It updates the proxy configuration to handle routing as specified in the Ingress resource.

### 4. Traffic Routing
- Incoming traffic to the Kubernetes cluster hits the proxy/load balancer managed by the Ingress Controller.
- The proxy uses the configured rules to route traffic to the appropriate service within the cluster.




## Interview Questions

- **What is the difference between a container, pod, and deployment?**
- **What is the difference between a deployment and a replica set?**

