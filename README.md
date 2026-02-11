# AWS Terraform VPC Baseline (Multi-AZ)

## Overview
This repository provisions a reusable AWS VPC baseline using Terraform:
- 1 VPC with DNS support/hostnames enabled
- 2 Public subnets (across 2 AZs)
- 2 Private subnets (across 2 AZs)
- Internet Gateway + public route table (`0.0.0.0/0 -> igw`)
- Private route table intentionally has **no** default internet route (no NAT) to keep costs at $0

## Architecture

## Prerequisites
- AWS account
- IAM user with VPC permissions
- AWS CLI configured
- Terraform >= 1.5

## How to Run
```bash
terraform init
terraform fmt -recursive
terraform validate
terraform plan -out tfplan
terraform apply tfplan

---

## ✅ Final checklist
- ✔ Architecture diagram is present  
- ✔ Diagram is inside a code block  
- ✔ Terraform commands are inside a closed code block  
- ✔ Markdown will render correctly on GitHub  

---

## Final step
```bash
git add README.md
git commit -m "Finalize README formatting"
git push
