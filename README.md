# Automate AWS Infrastructure with Terraform

## 📘 Project Overview

This project demonstrates how to automate the provisioning of a full AWS infrastructure setup using Terraform. It also includes deploying a Docker container (running NGINX) to an EC2 instance. This project was created as part of the "Terraform" module in the DevOps Bootcamp by TechWorld with Nana.

---

## ⚙️ Technologies Used

- **Terraform**
- **AWS** (EC2, VPC, Subnet, Route Table, Internet Gateway, Security Group)
- **Docker**
- **Linux**
- **Git**

---

## 🚀 What This Project Does

- Provisions the following AWS resources using Terraform:

  - VPC
  - Subnet
  - Route Table
  - Internet Gateway
  - EC2 Instance
  - Security Group

- Uses a **user data shell script** to:

  - Install Docker on the EC2 instance
  - Start the Docker service
  - Add the EC2 user to the Docker group
  - Run an NGINX container on port 8080

---

## 📜 entry.script.sh — Provisioning Script

Nana had me create a new file in the Terraform project called `entry.script.sh`, with the following content:

```bash
#!/bin/bash
sudo yum update && sudo yum install -y docker
sudo systemctl start docker
sudo usermod -aG docker ec2-user
docker run -p 8080:80 nginx
```

### 🔍 What this script does:

1. `sudo yum update && sudo yum install -y docker`

   - Updates the package list and installs Docker on the EC2 instance

2. `sudo systemctl start docker`

   - Starts the Docker service

3. `sudo usermod -aG docker ec2-user`

   - Adds the default EC2 user to the Docker group so Docker commands can be run without `sudo`

4. `docker run -p 8080:80 nginx`

   - Pulls and runs the official NGINX Docker image and exposes it on port 8080

This script is executed automatically by the EC2 instance during launch via the Terraform `user_data` directive.

---

## 🧪 Commands Used

### Initialize the Terraform project

```bash
terraform init
```

> Initializes the working directory containing Terraform configuration files and downloads the required providers.

### Preview changes

```bash
terraform plan
```

> Shows the execution plan and allows you to verify which resources will be created, updated, or destroyed.

### Apply the configuration

```bash
terraform apply
```

> Applies the changes required to reach the desired state of the configuration.

### Destroy the infrastructure

```bash
terraform destroy
```

> Destroys the infrastructure and resources defined in the Terraform configuration.

---

## 🧾 Sample Output (after `terraform apply`)

```
Apply complete! Resources: 7 added, 0 changed, 0 destroyed.

Outputs:
public_ip = "3.121.45.100"
```

You can now access the NGINX web server on: `http://<public_ip>:8080`

---

## 💡 Key Takeaways from Nana’s Lesson

- Terraform uses a **declarative syntax** to define infrastructure: you declare what the final state should be, and Terraform figures out how to get there.
- Terraform is **idempotent** — applying the same configuration multiple times results in no additional changes.
- By combining Terraform with AWS and Docker, you can fully automate end-to-end infrastructure and basic application deployment.
- Resource dependencies are automatically handled by Terraform.
- You can replicate entire environments (dev, staging, prod) by reusing or tweaking `.tf` files.

---

## 📂 Folder Structure

```
Terraform/
├── main.tf
├── providers.tf
├── terraform.tfstate
├── terraform.tfstate.backup
├── terraform-dev.tfvars
├── .terraform.lock.hcl
├── .gitignore
├── entry.script.sh
```

---

## 🧼 Notes

> Make sure to add a `.gitignore` file to exclude sensitive files like `terraform.tfstate`, `.terraform/`, and `*.tfvars`. You can use a template from the Terraform GitHub `.gitignore` best practices.

---

## 👤 Author

GitHub: [shlaskin101](https://github.com/shlaskin101)
