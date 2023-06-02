# Amazon EKS

This repository provides code for getting your very own Kubernetes cluster set up on AWS.

On AWS, in order to get your own Kubernetes cluster you use their [Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/).

It is well worth having a read over the EKS material and the further reading below if you want to understand greater detail of EKS.

As with anything on AWS we can create the service using the AWS CLI or the UI management console but in this repository you'll be using Terraform to provision your cluster.

The code in this guide is based upon the official Terraform EKS guide (link in the further reading)

## Instructions

**🗒️ NOTE:** To avoid complication, of having to setup remote state, this repository uses local state (instead of remote state) for Terraform.

### Explore the code

Have a read through the Terraform code.

To provision the EKS cluster it makes use of the [EKS official module](https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest)

### Apply terraform configuration

Once you have authenticated Terraform with your AWS account you can apply the configuration

Run the usual `init` followed by `plan` and then `apply` with Terraform

Then we wait....It can take some time for the cluster to be made so make sure your laptop remains active with an internet connection

This can take up to 30 minutes so use this time to checkout the [further reading resources](#further-reading)

### Connecting to the cluster with kubectl

Once your cluster has been provisioned it is time to configure the `kubectl` tool.

We can use the AWS command line to do this alongside the outputs of the terraform code. Run the following command (notice how it uses the terraform outputs as inputs to the AWS command):

```
aws eks --region $(terraform output -raw region) update-kubeconfig \
    --name $(terraform output -raw cluster_name)
```

You might want to verify that `kubectl` can connect - try something like `kubectl get services` or `kubectl get nodes`

### Deploying a container

Now let's try deploying a container and seeing if you can access it.

Using previous knowledge create a new directory to hold your kubernetes YAML files and create a deployment and service within that directory.

Try to get NGiNX deployed and exposed to the public internet via a service

**💡 HINT:** Have a look at type **LoadBalancer** for your service or even go further and have a read over getting services exposed via Kubernetes ingress. The further reading on exposing services might help.

## Tearing things down

It's worth renewing the AWS credentials before you destroy just in case they might have expired prior to you running destroy.

Firstly make sure to remove any kubernetes services you might have deployed by using the command (replacing SERVICE_FILE_NAME with whatever you have called the file):

```
kubectl delete -f SERVICE_FILE_NAME.yaml
```

This is because if you have used `type: LoadBalancer` then it will have provisioned an AWS ALB under the hood which we need to remove before destroying the cluster.

You should then be able to run `terraform destroy` to remove all the infrastructure.

If you find the terraform destroy works for a period of time then fails, you can run destroy again and it should tidy remaining aspects up.

### Submission process

1. Fork this repository

2. Work through the instructions

3. Complete the SOLUTION.md file

4. Destroy your cluster as indicated by the "Tearing things down" section

5. Share link to your GitHub repository

## Further reading

[Amazon EKS Explained](https://www.youtube.com/watch?v=E956xeOt050)

[Terraform EKS Guide](https://developer.hashicorp.com/terraform/tutorials/kubernetes/eks)

[Exposing Kubernetes services on EKS](https://aws.amazon.com/blogs/containers/exposing-kubernetes-applications-part-1-service-and-ingress-resources/)
