# 04 – Security Groups and Network ACLs

## Objective
Understand AWS network access controls and design least-privilege rules.

## Security Groups
A Security Group is a stateful virtual firewall associated with an ENI/resource.
- Controls inbound and outbound traffic.
- Supports allow rules, not explicit deny rules.
- Return traffic for an allowed connection is automatically permitted.
- Can reference another Security Group for tier-to-tier access.

## Network ACLs
A Network ACL is a stateless subnet-level filter.
- Separate inbound and outbound rules.
- Supports allow and deny rules.
- Evaluates rules in ascending rule-number order; first matching rule applies.
- Return traffic must be explicitly allowed in the relevant direction.

## Comparison

| Feature | Security Group | Network ACL |
|---|---|---|
| Scope | ENI/resource | Subnet |
| Stateful | Yes | No |
| Deny rules | No | Yes |
| Rule evaluation | All applicable rules | Lowest matching rule number |
| Return traffic | Automatically allowed | Must be explicitly allowed |

## Suggested Tier Rules

| Security Group | Inbound | Source |
|---|---|---|
| `web-server-sg` | HTTP 80, HTTPS 443 | Intended clients |
| `web-server-sg` | SSH 22 | Administrator IP `/32` only |
| `app-server-sg` | App port, e.g. 8080 | `web-server-sg` |
| `database-sg` | DB port, e.g. MySQL 3306 | `app-server-sg` |

Only open ports required by deployed workloads. Never expose SSH or database ports to the entire internet.

## CLI Verification
```bash
aws ec2 describe-security-groups \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" --output table

aws ec2 describe-network-acls \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" --output table
```

## Learning Outcome
Understood stateful versus stateless filtering and designed least-privilege access between network tiers.
