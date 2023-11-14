# Amazon EKS

This repository provides code for getting your very own Kubernetes cluster set up on AWS.

On AWS, in order to get your own Kubernetes cluster you use their [Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/).

It is well worth having a read over the EKS material and the further reading below if you want to understand greater detail of EKS.

As with anything on AWS we can create the service using the AWS CLI or the UI management console but in this repository you'll be using Terraform to provision your cluster.

The code in this guide is based upon the official Terraform EKS guide (link in the further reading)

## Instructions

### Explore the code

Have a read through the Terraform code.

To provision the EKS cluster it makes use of the [EKS official module](https://registry.terraform.io/modules/terraform-aws-modules/eks/aws/latest)

### 1. Apply terraform configuration

Once you have authenticated Terraform with your AWS account you can apply the configuration

Run the usual `init` followed by `plan` and then `apply` with Terraform

Then we wait....It can take some time for the cluster to be made so make sure your laptop remains active with an internet connection. If you have spotty internet then reach out to your mentor for a solution.

This can take up to 30 minutes so use this time to checkout the [further reading resources](#further-reading)

### 2. Connecting to the cluster with kubectl

Once your cluster has been provisioned it is time to configure the `kubectl` tool.

We can use the AWS command line to do this alongside the outputs of the terraform code. Run the following command (notice how it uses the terraform outputs as inputs to the AWS command):

```
aws eks --region $(terraform output -raw region) update-kubeconfig \
    --name $(terraform output -raw cluster_name)
```

To verify if you've connected correctly run `kubectl get nodes`. You should see something similar to this;

```bash
NAME                                          STATUS   ROLES    AGE    VERSION
ip-192-168-1-2.eu-west-2.compute.internal       Ready    <none>   3d1h   v1.21.4-eks-xxxxxxxx
```

### 3. Deploying a container

Now let's try deploying a container to our EKS cluster and see if you can access it.

Using previous knowledge create a new directory to hold your kubernetes YAML files and create a deployment and service within that directory.

Deploy an NGiNX container exposed to the public internet via a LoadBalancer service. This can take a couple of minutes to be reachable as it is provisioning a Load Balancer on AWS for you.

📷 - Take a screenshot of your browser, showing the Nginx page and the URL you requested to reach it, and place it inside the repo

### 4. Once more with feeling

Deploy the smart home microservices that you were working on yesterday to your cluster on EKS.

### 5. Do it again

Deploy the bookstore frontend and backend to your cluster and get them connected.

## Tearing things down

It's worth renewing the AWS credentials before you destroy just in case they might have expired prior to you running destroy.

Firstly make sure to remove any kubernetes services you might have deployed by using the command (replacing SERVICE_FILE_NAME with whatever you have called the file):

```
kubectl delete -f SERVICE_FILE_NAME.yaml
```

This is because if you have used `type: LoadBalancer` then it will have provisioned an AWS ALB under the hood which we need to remove before destroying the cluster.

You should then be able to run `terraform destroy` to remove all the infrastructure.

If you find the terraform destroy works for a period of time then fails, you can run destroy again and it should tidy remaining aspects up.

## Further reading

[Amazon EKS Explained](https://www.youtube.com/watch?v=E956xeOt050)

[Terraform EKS Guide](https://developer.hashicorp.com/terraform/tutorials/kubernetes/eks)

[Exposing Kubernetes services on EKS](https://aws.amazon.com/blogs/containers/exposing-kubernetes-applications-part-1-service-and-ingress-resources/)
