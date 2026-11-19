# 02 – Subnets, CIDR and IP Addressing

## Objective
Create non-overlapping subnets and understand AWS IPv4 address allocation.

## Address Plan

VPC CIDR: `10.0.0.0/16`

| Subnet | CIDR | Intended tier |
|---|---|---|
| `web-subnet` | `10.0.1.0/24` | Web |
| `database-subnet` | `10.0.2.0/24` | Database |
| `app-private-subnet` | `10.0.3.0/24` | Application |

Choose an Availability Zone that exists in your selected AWS Region. A subnet belongs to one Availability Zone.

## CIDR Basics
A `/24` contains 256 total IPv4 addresses. AWS reserves five addresses in each subnet: network, VPC router, DNS, future use, and the final address. Therefore, an AWS `/24` has **251 usable IPv4 addresses**.

| CIDR | Total | AWS-usable |
|---|---:|---:|
| `/24` | 256 | 251 |
| `/25` | 128 | 123 |
| `/26` | 64 | 59 |
| `/28` | 16 | 11 |

## Important Rules
- Subnet CIDR must be inside the VPC CIDR.
- Subnet CIDRs in one VPC must not overlap.
- A subnet is not public just because of its name; routing determines this.
- Auto-assigning a public IPv4 address does not itself create an internet route.

## Create Subnets
1. Open VPC → Subnets → Create subnet.
2. Select `devops-custom-vpc`.
3. Create the three subnets using the address plan above.
4. Select valid Availability Zones.

## CLI Verification
```bash
aws ec2 describe-subnets \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" \
  --query "Subnets[*].[SubnetId,CidrBlock,AvailabilityZone,AvailableIpAddressCount]" \
  --output table
```
Replace `YOUR_VPC_ID` with your actual VPC ID.

## Learning Outcome
Created three non-overlapping subnets and understood CIDR sizing and AWS-reserved IPv4 addresses.
