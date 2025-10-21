# 🏗️ Terraform GitHub Actions Workflows

This repository demonstrates how to automate Terraform deployments and cleanups using **GitHub Actions** as part of the *“3.1 Introduction to DevSecOps & Workflows”* hands-on activity.

---

## 📘 Overview

You will create and manage Terraform infrastructure using two workflows:

1. **terraform-apply.yaml** — to deploy resources automatically on push.  
2. **terraform-destroy.yaml** — to manually destroy the same resources when needed.

Both workflows integrate with AWS via repository secrets.

---

## 🧩 Prerequisites

Before running the workflows:

1. **Create GitHub Secrets**
   - `AWS_ACCESS_KEY_ID`  
   - `AWS_SECRET_ACCESS_KEY`

   > Go to **Settings → Secrets and Variables → Actions → New Repository Secret**.

2. **Terraform Backend Setup**
   - Your Terraform files must be located at the **root** of the repository.
   - Use an S3 backend for remote state:
     ```hcl
     terraform {
       backend "s3" {
         bucket = "<your-s3-bucket-name>"
         key    = "terraform/state.tfstate"
         region = "ap-southeast-1"
       }
     }
     ```

---

## ⚙️ Workflow 1 — terraform-apply.yaml

This workflow automatically **deploys** your infrastructure on every push to the `main` branch.

**Trigger:** `on: push`

It performs the following steps:
1. Checks out the repository  
2. Configures AWS credentials  
3. Initializes Terraform (`terraform init`)  
4. Plans the deployment (`terraform plan`)  
5. Applies changes automatically (`terraform apply --auto-approve`)

---

## ⚙️ Workflow 2 — terraform-destroy.yaml

This workflow allows you to **manually destroy** Terraform-managed resources.

**Trigger:** `on: workflow_dispatch` (manual trigger)

It includes:
- A safety confirmation (`Type YES to confirm destroy`)
- AWS authentication
- `terraform plan -destroy`
- `terraform destroy -auto-approve`

To run it:
1. Go to your GitHub repository → **Actions tab**  
2. Select **Terraform Destroy (Manual Trigger)**  
3. Click **Run workflow**, type `YES`, and confirm  

---

## 🧠 Notes

- Keep the backend configuration consistent between workflows.  
- Ensure the same AWS region and credentials are used for both apply and destroy.  
- Never store credentials directly in your Terraform files — always use GitHub Secrets.  

---

## 📂 Directory Structure

```
.
├── main.tf
├── variables.tf
├── outputs.tf
└── .github/
    └── workflows/
        ├── terraform-apply.yaml
        └── terraform-destroy.yaml
```

---

## ✅ Example Output

When successful, the **Actions tab** will display:
```
Terraform Apply: Completed Successfully ✅
Terraform Destroy: All resources deleted ✅
```
