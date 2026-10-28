
# AWS Terraform VPC Baseline (Multi-AZ)

## Overview

This project uses Terraform to provision a reusable AWS Virtual Private Cloud (VPC) baseline across two Availability Zones.

It demonstrates Infrastructure as Code (IaC), subnet design, routing, and network isolation.

## Infrastructure Components

- 1 VPC with DNS support and DNS hostnames enabled
- 2 public subnets across 2 Availability Zones
- 2 private subnets across 2 Availability Zones
- 1 Internet Gateway
- 1 public route table with a default route to the Internet Gateway
- 1 private route table with no default internet route
- Route table associations for all four subnets
- No NAT Gateway provisioned

## Architecture

```text
                   AWS Region (eu-central-1)
+----------------------------------------------------+
|                 VPC 10.0.0.0/16                    |
|                                                    |
|  +----------------------+  +--------------------+  |
|  | Availability Zone 1  |  | Availability Zone 2|  |
|  |                      |  |                    |  |
|  | Public Subnet 1      |  | Public Subnet 2    |  |
|  | 10.0.0.0/24          |  | 10.0.1.0/24        |  |
|  |                      |  |                    |  |
|  | Private Subnet 1     |  | Private Subnet 2   |  |
|  | 10.0.100.0/24        |  | 10.0.101.0/24      |  |
|  +----------------------+  +--------------------+  |
|                                                    |
| Public Route Table: 0.0.0.0/0 -> Internet Gateway  |
| Private Route Table: Local VPC route only          |
+----------------------------------------------------+
                         |
                  Internet Gateway
                         |
                      Internet
```

The public subnets use a route table with a default route to the Internet Gateway. The private subnets have no default route to the internet.

Public subnets are configured to automatically assign public IPv4 addresses to instances launched into them.

## Prerequisites

- AWS account
- AWS credentials with appropriate VPC permissions
- AWS CLI configured
- Terraform installed

## Deployment

Clone the repository:

```bash
git clone https://github.com/xlyatif/aws-terraform-vpc-baseline.git
cd aws-terraform-vpc-baseline
```

Initialize Terraform:

```bash
terraform init
```

Format and validate the configuration:

```bash
terraform fmt -recursive
terraform validate
```

Generate and review the execution plan:

```bash
terraform plan -out=tfplan
```

Apply the configuration:

```bash
terraform apply tfplan
```

## Terraform Outputs

The configuration provides the following outputs:

- `vpc_id`
- `public_subnet_ids`
- `private_subnet_ids`
- `public_route_table_id`
- `private_route_table_id`

View the outputs using:

```bash
terraform output
```

## Cost Considerations

This baseline does not deploy NAT Gateways, EC2 instances, or other compute resources.

Standard VPC components such as subnets, route tables, and Internet Gateways generally have no direct hourly charge. Additional AWS resources and network traffic may incur charges.

## Cleanup

To remove the resources created by this Terraform configuration:

```bash
terraform destroy
```

Review the proposed destruction before confirming.

## Learning Outcomes

This project demonstrates practical experience with:

- Provisioning AWS networking infrastructure using Terraform
- Designing public and private subnets across multiple Availability Zones
- Configuring Internet Gateway routing
- Associating route tables with subnets
- Understanding private subnet isolation
- Managing infrastructure lifecycle through Terraform
