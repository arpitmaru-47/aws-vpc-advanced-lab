# 03 – Route Tables, Internet Gateway and NAT Gateway

## Objective
Understand AWS traffic routing and configure an Internet Gateway route for the web subnet.

## Route Tables
A route table contains destination and target entries. Every VPC route table includes a local route for communication within the VPC CIDR.

Example public route table:

| Destination | Target |
|---|---|
| `10.0.0.0/16` | `local` |
| `0.0.0.0/0` | Internet Gateway |

`0.0.0.0/0` represents all IPv4 destinations not matched by a more specific route.

## Internet Gateway (IGW)
An IGW is attached to a VPC and is a route target for internet traffic. Direct IPv4 internet connectivity for an EC2 instance also requires public IPv4/EIP addressing and suitable security controls.

## NAT Gateway
A NAT Gateway allows private-subnet resources to initiate outbound internet connections. It does not allow unsolicited inbound internet connections. NAT Gateway has hourly and data-processing charges.

## Lab Configuration
- IGW: `devops-igw`
- Public route table: `public-rt`
- Associate `web-subnet` with `public-rt`.
- Add `0.0.0.0/0 → devops-igw`.

## Console Procedure
1. VPC → Internet Gateways → Create `devops-igw`.
2. Attach it to `devops-custom-vpc`.
3. Create route table `public-rt` in that VPC.
4. Add route `0.0.0.0/0` targeting the IGW.
5. Associate `web-subnet` with `public-rt`.

## Private Route Table
For this no-NAT lab, the private route table should retain the local route and must not have a default route directly to the IGW. Associate the application and database subnets with it.

## CLI Verification
```bash
aws ec2 describe-internet-gateways \
  --filters "Name=attachment.vpc-id,Values=YOUR_VPC_ID"

aws ec2 describe-route-tables \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" --output table
```

## Learning Outcome
Configured an IGW and public route table and understood private-subnet routing.
