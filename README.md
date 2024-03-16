# AWS EKS Smart Home Services Deployment

## Overview
This repository contains code and configuration files to deploy smart home services on Amazon Elastic Kubernetes Service (EKS) using Terraform for infrastructure provisioning.

## Prerequisites
Before proceeding, ensure the following:
- Terraform is installed on your local machine.
- You have authenticated with your AWS account.
- Basic knowledge of Kubernetes and Docker.

## Repo
The repository includes:
- Terraform code for provisioning the EKS cluster.
- YAML files for deploying smart home services.
- Ingress configuration file (`ingress.yaml`).

## Provisioning Your Cluster
To provision the EKS cluster:
1. Clone this repository to your local machine.
2. Navigate to the `terraform` directory.
3. Authenticate Terraform with your AWS account.
4. Run `terraform init`, `terraform plan`, and `terraform apply`.
5. Wait for the cluster provisioning to complete (approximately 30 minutes).

## Connecting To The Cluster With Kubectl
Configure `kubectl` to connect to the provisioned cluster:
1. Use AWS CLI alongside Terraform outputs.
2. Run the provided command to update `kubeconfig`.
3. Verify the connection by running `kubectl get nodes`.

## Deploying Smart Home Services
Deploy smart home services to the EKS cluster:
1. Create a new directory for Kubernetes YAML files.
2. Use the provided YAML files to deploy services (e.g., NGINX, smart home applications).
3. Verify service accessibility using `kubectl get services`.

## Implementing Ingress
Implement Ingress to expose services publicly:
1. Convert services to use ClusterIP instead of LoadBalancer.
2. Deploy `nginx-ingress-controller` on the cluster.
3. Configure Ingress rules in `ingress.yaml`.
4. Verify service accessibility via Ingress.

## Tearing Down
To tear down the cluster:
1. Ensure all services are working as expected.
2. Check and remove the Load Balancer created for NGINX.
3. Navigate to the `terraform` directory.
4. Run `terraform destroy` to delete the EKS cluster.

By following these instructions, you can successfully deploy and manage smart home services on AWS EKS.
