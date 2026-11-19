
# 01 – VPC Fundamentals

## Objective
Create and understand an isolated network in Amazon VPC.

## What is a VPC?
A VPC is a logically isolated virtual network in an AWS Region. It contains subnets, route tables, gateways, and network security controls.

## Lab Configuration

| Setting | Value |
|---|---|
| VPC name | `devops-custom-vpc` |
| IPv4 CIDR | `10.0.0.0/16` |
| Tenancy | Default |

## Key Concepts
- **VPC:** Overall virtual network boundary.
- **CIDR:** Defines the IP address range.
- **Subnet:** Smaller IP range in one Availability Zone.
- **Route table:** Directs network traffic.
- **Internet Gateway:** Provides an internet route target.
- **Security Group:** Stateful firewall associated with an ENI/resource.
- **Network ACL:** Stateless subnet-level traffic filter.

## Create the VPC
1. Open VPC → Your VPCs → Create VPC.
2. Select **VPC only**.
3. Name: `devops-custom-vpc`.
4. IPv4 CIDR: `10.0.0.0/16`.
5. Keep tenancy as Default and create.

## CLI Verification
```bash
aws ec2 describe-vpcs \
  --filters "Name=tag:Name,Values=devops-custom-vpc" \
  --query "Vpcs[*].[VpcId,CidrBlock,State]" --output table
```

## Notes
A VPC itself has no hourly VPC charge. Creating a VPC does not automatically make its subnets public.

## Learning Outcome
Created and inspected a custom VPC and understood its main networking components.
