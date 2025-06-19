
Accessing EKS Cluster from EC2
==============================

1. Introduction
---------------
- The EKS cluster was previously created using Terraform.
- This section explains how to access the EKS cluster from an EC2 instance rather than a local machine.
- We'll use an EC2 instance (as introduced earlier in the project) to interact with the EKS cluster.

2. Why Not Use Local Machine?
-----------------------------
- While developing Terraform files, the local machine was used due to IDE support (e.g., VS Code).
- The EC2 instance does not offer the same developer experience, but it simulates a more production-like environment.

3. Issue with kubectl
---------------------
- Running `kubectl get nodes` may not display the EKS cluster nodes.
- This is because `kubectl` requires a kubeconfig file that defines which Kubernetes cluster it should connect to.

4. kubectl and kubeconfig Overview
----------------------------------
- `kubectl` uses a `kubeconfig` file to manage access to one or more Kubernetes clusters.
- The file contains **contexts**, each pointing to a specific cluster.
- You can switch between contexts to manage different clusters.

5. Checking Current Context
---------------------------
- Run `kubectl config view` to inspect current kubeconfig contents.
- Run `kubectl config current-context` to see which cluster context is active (will be null if not set).
- Use `kubectl config use-context <context-name>` to switch between clusters.

6. How to Update the kubeconfig File
------------------------------------
To connect to your EKS cluster, AWS CLI must be installed and configured.

a. Install unzip (if not already installed):
   ```bash
   sudo apt install unzip
b. Install AWS CLI
------------------
Run the following commands to install the AWS CLI:

curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

c. Configure AWS CLI with your IAM credentials
----------------------------------------------
After installation, run:

aws configure

Enter your AWS Access Key ID, Secret Access Key, region, and default output format when prompted.

Configuring Access to the EKS Cluster
-------------------------------------
Use the following command to update your kubeconfig file so kubectl knows how to connect to your EKS cluster:

aws eks --region <your-region> update-kubeconfig --name <your-cluster-name>

Example:

aws eks --region us-west-2 update-kubeconfig --name my-eks-cluster

This command will create a new context in your kubeconfig file pointing to the specified EKS cluster.

Verifying the Connection
-------------------------
a. Verify that the context has been added:

kubectl config view
kubectl config current-context

b. Confirm access to the cluster:

kubectl get nodes

If successful, this will display the nodes currently registered in your EKS cluster.

Summary
-------
- Connecting kubectl to your EKS cluster requires updating the kubeconfig file via the AWS CLI.
- This enables context-based switching between multiple Kubernetes clusters.
- Once configured, the EC2 instance can be used to interact with your EKS cluster just like a local development environment.
