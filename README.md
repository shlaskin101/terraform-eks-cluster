# Terraform & AWS EKS

## Project Overview

This project demonstrates how to automate the provisioning of an Amazon EKS (Elastic Kubernetes Service) cluster using Terraform. It includes configuring infrastructure with both public and private subnets, provisioning worker nodes, and deploying a simple NGINX application to verify the setup. The approach follows best practices for modular, reusable infrastructure as taught by Nana in the TechWorld with Nana bootcamp.

---

## Technologies Used

* **Terraform**
* **AWS EKS**
* **Docker**
* **Linux**
* **Git**

---

## Infrastructure Design

* **3 Public Subnets** (each in a different AZ, connected to an Internet Gateway)
* **3 Private Subnets** (each in a different AZ, routed through a NAT Gateway)
* **EKS Cluster** with worker nodes (Amazon Linux AMIs)

---

## Prerequisites

Make sure the following are installed:

* AWS CLI
* kubectl
* aws-iam-authenticator

---

## Key Terraform Configurations

In `eks-cluster.tf`, ensure the following line is included:

```hcl
cluster_endpoint_public_access = true
```

This allows you to access your EKS cluster from your local machine.

---

## Terraform Workflow

```bash
terraform init
terraform plan
terraform apply --auto-approve
```

---

## Configure kubectl to Connect to EKS

```bash
aws eks update-kubeconfig --name myapp-eks-cluster --region us-east-2
```

Output:

```
Added new context arn:aws:eks:us-east-2:193668171416:cluster/myapp-eks-cluster to /Users/stevenlaskin/.kube/config
```

---

## Validate EKS Cluster Nodes

```bash
kubectl get node
```

Output:

```
NAME                                       STATUS   ROLES    AGE   VERSION
ip-10-0-1-108.us-east-2.compute.internal   Ready    <none>   24m   v1.27.16-eks-aeac579
...
```

---

## Deploy NGINX to the Cluster

```bash
kubectl apply -f ~/nginx-config3.yaml
```

Then monitor pod deployment:

```bash
kubectl get pod -w
```

Output:

```
NAME                    READY   STATUS    RESTARTS   AGE
nginx-55f598f8d-55nlp   1/1     Running   0          9s
```

---

## Access the NGINX Web Page

Retrieve the external LoadBalancer DNS:

```bash
kubectl get svc
```

Output:

```
NAME         TYPE           CLUSTER-IP      EXTERNAL-IP                                                               PORT(S)        AGE
nginx        LoadBalancer   172.20.37.167   a5fa0d6b35759423daebbb9533fc2cd8-1646228942.us-east-2.elb.amazonaws.com   80:31476/TCP   43s
```

Visit the DNS name in a browser to confirm deployment:

```
Welcome to nginx!
```

---

## Tear Down Infrastructure

```bash
terraform destroy --auto-approve
terraform state list  # Ensure resources were destroyed
```

---

## Final Thoughts

This project successfully demonstrates end-to-end automation of an AWS EKS cluster with Terraform. Nana emphasizes the importance of breaking infrastructure into reusable modules and validating deployments through simple service exposure like NGINX before expanding.

---

## Author

Steven Laskin
DevOps Engineer in Training
