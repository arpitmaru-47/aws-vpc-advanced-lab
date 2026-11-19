# 05 – VPC Peering and Connectivity

## Objective
Understand private communication between two VPCs using VPC Peering.

## What is VPC Peering?
VPC Peering is a one-to-one networking connection between two VPCs. Resources can communicate using private IP addresses when the peering connection, routes, and security rules are configured.

## Requirements and Limitations
- VPC CIDR ranges must not overlap.
- The requester creates a peering request; the accepter accepts it.
- Routes must be added to relevant route tables on both sides.
- Security Groups and NACLs must allow the required traffic.
- Peering is not transitive: A↔B and B↔C do not automatically connect A↔C.
- An Internet Gateway is not required for peering.

## Example

| VPC | CIDR | Role |
|---|---|---|
| VPC A | `10.0.0.0/16` | Application |
| VPC B | `172.16.0.0/16` | Shared services |

Each VPC needs a route to the other VPC CIDR through the peering connection.

## Lab Status
The lab inspected VPCs and CIDR ranges to understand peering compatibility. Do not claim a peering connection was deployed unless it was created, accepted, and verified.

## CLI Inspection
```bash
aws ec2 describe-vpcs \
  --query "Vpcs[*].[VpcId,CidrBlock,IsDefault]" --output table

aws ec2 describe-vpc-peering-connections --output table
```

## Cost Notes
There is no hourly charge for the peering connection itself, but data transfer charges may apply.

## Learning Outcome
Understood private VPC connectivity, CIDR overlap restrictions, route requirements, and non-transitive behavior.
