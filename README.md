
# Secure Multi-Tier AWS VPC Architecture

## Project Overview

This project demonstrates the design and configuration of a secure AWS Virtual Private Cloud (VPC) using AWS networking services.

The objective is to understand how to build an isolated network, divide it into subnets, control network traffic, configure private connectivity, and monitor network activity.

## AWS Services Used

- Amazon VPC
- Subnets
- Route Tables
- Internet Gateway (IGW)
- Security Groups
- Network ACLs
- VPC Peering (concept and inspection)
- VPC Endpoints – S3 Gateway Endpoint
- VPC Flow Logs
- Amazon CloudWatch Logs

## Network Architecture

**VPC CIDR:** `10.0.0.0/16`

| Subnet | CIDR | Purpose |
|---|---|---|
| Web Subnet | `10.0.1.0/24` | Public-facing network tier |
| Database Subnet | `10.0.2.0/24` | Private database network tier |
| Application Subnet | `10.0.3.0/24` | Private application network tier |

## Architecture Design

- A custom VPC provides an isolated network.
- The Web subnet uses a route table with an Internet Gateway route.
- Application and Database subnets use a private route table.
- Separate Security Groups define access between network tiers.
- An S3 Gateway Endpoint provides private connectivity to Amazon S3 through its associated route table.
- VPC Flow Logs capture network traffic metadata for monitoring and troubleshooting.

See the `architecture/` directory for the architecture diagram and design notes.

## Key Concepts Demonstrated

1. VPC architecture and CIDR planning
2. Subnet creation and IP addressing
3. Route tables and Internet Gateway
4. Security Groups and Network ACLs
5. VPC Peering concepts
6. VPC Endpoints and AWS PrivateLink
7. VPN, Transit Gateway concepts and VPC Flow Logs
8. Secure multi-tier VPC network design

## Repository Structure

- `architecture/` – Network architecture diagram and design explanation.
- `docs/` – Chapter-wise technical documentation.
- `screenshots/` – AWS Console configuration evidence.
- `commands/` – AWS CLI verification commands.

## Security Considerations

- SSH access should be restricted to an administrator's IP address.
- Database access should be allowed only from the application tier.
- Private subnets should not have a direct Internet Gateway route.
- AWS credentials, access keys, account IDs and sensitive information must never be committed.
- Security Groups should follow the principle of least privilege.

## Cost Considerations

The project avoids unnecessary paid resources during the networking lab.

Potential charges can arise from:
- NAT Gateway
- Interface VPC Endpoints
- Public IPv4 addresses
- CloudWatch Logs ingestion and storage
- Data transfer

## Verification

The VPC, subnet, routing, security, endpoint and Flow Logs configurations were checked during the hands-on lab.

Screenshots and relevant CLI outputs are included as project evidence.

## Learning Outcome

This project provides hands-on experience with AWS networking fundamentals and the design of a secure multi-tier VPC architecture.

## Disclaimer

This is an educational hands-on networking lab. The architecture demonstrates network segmentation and security configuration; it should not be described as a fully deployed production application unless application workloads, availability, recovery and operational controls are also implemented.
