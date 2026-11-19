# 07 – VPN, Transit Gateway and VPC Flow Logs

## Objective
Understand enterprise connectivity and monitor VPC traffic metadata.

## Site-to-Site VPN
AWS Site-to-Site VPN creates encrypted IPsec tunnels between an on-premises network and AWS.

- **Customer Gateway:** Represents the customer-side VPN device or software endpoint.
- **Virtual Private Gateway:** AWS-side VPN endpoint option attached to a VPC.
- **Transit Gateway:** Can act as a hub for multiple VPCs and on-premises connections.

VPN requires configuration on both AWS and the customer network side.

## Transit Gateway
Transit Gateway is a network transit hub. It can connect multiple VPCs and on-premises networks using attachments and route tables. It supports hub-and-spoke designs. Attachments and data processing can incur charges.

## VPC Flow Logs
Flow Logs capture metadata about IP traffic to and from network interfaces, subnets, or VPCs.

Fields can include source/destination IPs, ports, protocol, packet/byte counts, and ACCEPT/REJECT action. Flow Logs do **not** capture packet payload contents.

## Lab Configuration
- Flow Log name: `devops-vpc-flowlogs`
- Resource: `devops-custom-vpc`
- Traffic filter: `ALL`
- Destination: CloudWatch Logs
- Example log group: `/aws/vpc/devops-flowlogs`

A CloudWatch Logs destination requires an appropriately configured IAM role. Use a short retention period for a temporary lab.

## CLI Verification
```bash
aws ec2 describe-flow-logs \
  --filter Name=resource-id,Values=YOUR_VPC_ID \
  --query "FlowLogs[*].[FlowLogId,FlowLogStatus,TrafficType,LogDestinationType]" \
  --output table
```

Expected active status: `ACTIVE`.

## Cost Notes
CloudWatch Logs ingestion/storage may incur charges. VPN and Transit Gateway resources may also incur charges. Do not claim VPN or Transit Gateway was deployed unless you created and verified it.

## Learning Outcome
Understood Site-to-Site VPN, Transit Gateway, and Flow Logs for network monitoring.
