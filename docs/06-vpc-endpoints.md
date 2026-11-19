# 06 – VPC Endpoints and AWS PrivateLink

## Objective
Understand private access from a VPC to supported AWS services.

## VPC Endpoint
A VPC Endpoint provides a private connection between a VPC and a supported AWS service or endpoint service. IAM policies, endpoint policies, and service resource policies can still restrict access.

## Endpoint Types

| Type | Description |
|---|---|
| Gateway Endpoint | Supports S3 and DynamoDB; adds routes to selected route tables |
| Interface Endpoint | Uses endpoint network interfaces with private IPs; powered by AWS PrivateLink |
| Gateway Load Balancer Endpoint | Provides a path to supported Gateway Load Balancer appliances |

## S3 Gateway Endpoint
An S3 Gateway Endpoint lets resources in selected route tables reach S3 without requiring a NAT Gateway or IGW for that S3 path.

Example endpoint name: `s3-gateway-endpoint`.

Example Mumbai service name: `com.amazonaws.ap-south-1.s3`. Select the service for your actual AWS Region.

## Console Procedure
1. VPC → Endpoints → Create endpoint.
2. Choose AWS services and search for the regional S3 service.
3. Choose Gateway endpoint type.
4. Select `devops-custom-vpc`.
5. Select the route tables that need S3 access.
6. Create and wait for state `Available`.

**Important:** If private application resources need S3 access, associate `private-rt` with the endpoint too. Associating only `public-rt` does not add the endpoint route to private subnets.

## CLI Verification
```bash
aws ec2 describe-vpc-endpoints \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID" \
  --query "VpcEndpoints[*].[VpcEndpointId,ServiceName,VpcEndpointType,State]" \
  --output table
```

## AWS PrivateLink
PrivateLink enables private connectivity to supported AWS services and privately published services. Interface VPC Endpoints are a common consumer access pattern.

## Cost Notes
S3 and DynamoDB Gateway Endpoints have no additional hourly endpoint charge. Interface Endpoints generally incur hourly and data-processing charges.

## Learning Outcome
Created and inspected an S3 Gateway Endpoint and learned Gateway versus Interface Endpoint differences.
