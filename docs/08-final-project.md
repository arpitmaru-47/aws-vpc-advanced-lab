# 08 – Final Project: Secure Multi-Tier VPC Architecture

## Objective
Design and configure a segmented AWS VPC for web, application, and database tiers. Practice subnetting, routing, network security, private AWS service access, and traffic monitoring.

## Address Plan

| Component | Name | CIDR / Setting | Intended purpose |
|---|---|---|---|
| VPC | `devops-custom-vpc` | `10.0.0.0/16` | Isolated network |
| Web subnet | `web-subnet` | `10.0.1.0/24` | Public-facing tier |
| Database subnet | `database-subnet` | `10.0.2.0/24` | Private data tier |
| Application subnet | `app-private-subnet` | `10.0.3.0/24` | Private application tier |
| IGW | `devops-igw` | Attached to VPC | Internet route target |
| Public route table | `public-rt` | `0.0.0.0/0 → IGW` | Web subnet route |
| Private route table | `private-rt` | Local route only in no-NAT lab | App and database subnets |
| S3 Endpoint | `s3-gateway-endpoint` | Gateway endpoint | Private S3 path via associated route tables |
| Flow Logs | `devops-vpc-flowlogs` | CloudWatch Logs | Traffic metadata monitoring |

## Conceptual Traffic Design
`Internet → IGW → Web Tier → Application Tier → Database Tier`

The web-to-application and application-to-database paths require actual workloads, routes where applicable, and matching Security Group rules. If no EC2 instances or database workloads were deployed, this is a network design rather than a running application.

## Route Table Design

### Public Route Table
- `10.0.0.0/16 → local`
- `0.0.0.0/0 → devops-igw`
- Associate with `web-subnet`.

### Private Route Table
- `10.0.0.0/16 → local`
- No direct default route to the IGW in this no-NAT lab.
- Associate with `app-private-subnet` and `database-subnet`.

General outbound internet access for private resources would require a separately designed egress path, such as a NAT Gateway, with associated charges.

## Security Group Design

| Security Group | Inbound access |
|---|---|
| `web-server-sg` | HTTP 80/HTTPS 443 from intended clients; SSH 22 only from administrator IP if needed |
| `app-server-sg` | Application port, e.g. 8080, from `web-server-sg` only |
| `database-sg` | Database port, e.g. MySQL 3306, from `app-server-sg` only |

Only enable rules required by deployed workloads. Use Security Group references for tier-to-tier access where supported.

## S3 Gateway Endpoint
Associate the endpoint with every route table whose subnet resources need the private S3 route. An association with only `public-rt` does not provide the endpoint route to private subnets.

## Verification Checklist
- [ ] Confirm VPC CIDR and state.
- [ ] Confirm all three subnet CIDRs and Availability Zones.
- [ ] Confirm IGW attachment.
- [ ] Confirm public route `0.0.0.0/0` targets the IGW.
- [ ] Confirm `web-subnet` is associated with `public-rt`.
- [ ] Confirm application and database subnets are associated with `private-rt`.
- [ ] Confirm private route table has no direct IGW default route.
- [ ] Inspect Security Group rules and sources.
- [ ] Confirm S3 Endpoint state and route table associations.
- [ ] Confirm Flow Logs status and destination.

## AWS CLI Verification
```bash
aws ec2 describe-vpcs --query "Vpcs[*].[VpcId,CidrBlock,State]" --output table

aws ec2 describe-subnets \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" --output table

aws ec2 describe-route-tables \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" --output table

aws ec2 describe-security-groups \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" --output table

aws ec2 describe-vpc-endpoints \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" --output table

aws ec2 describe-flow-logs \
  --filter Name=resource-id,Values=YOUR_VPC_ID --output table
```

Replace `YOUR_VPC_ID` with the actual VPC ID. Do not commit account IDs or sensitive output to a public repository.

## Cost and Cleanup
Potential charges may come from NAT Gateway, Interface Endpoints, public IPv4 addresses, CloudWatch Logs, data transfer, EC2, RDS, load balancers, VPN, and Transit Gateway.

VPCs, subnets, route tables, IGWs, and Security Groups have no separate hourly charge. Before deleting resources, check dependencies and whether logs or snapshots must be retained.

## Project Outcome
This lab demonstrates a segmented AWS VPC design and configuration. Describe it as a fully deployed multi-tier application only if web, application, and database workloads were deployed and tested end-to-end.
