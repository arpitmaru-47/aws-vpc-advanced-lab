

# VPC
aws ec2 describe-vpcs

# Subnets
aws ec2 describe-subnets \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID"

# Route Tables
aws ec2 describe-route-tables \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID"

# Internet Gateway
aws ec2 describe-internet-gateways

# Security Groups
aws ec2 describe-security-groups \
  --filters "Name=vpc-id,Values=YOUR_VPC_ID"

# VPC Endpoints
aws ec2 describe-vpc-endpoints

# VPC Flow Logs
aws ec2 describe-flow-logs
