

# VPC Architecture

```mermaid
flowchart TB
    Internet((Internet))
    IGW[Internet Gateway]

    subgraph VPC["AWS VPC - 10.0.0.0/16"]
        subgraph Public["Web Subnet - 10.0.1.0/24"]
            Web[Web Tier]
        end

        subgraph PrivateApp["Application Subnet - 10.0.3.0/24"]
            App[Application Tier]
        end

        subgraph PrivateDB["Database Subnet - 10.0.2.0/24"]
            DB[Database Tier]
        end

        S3[S3 Gateway Endpoint]
    end

    Internet --> IGW
    IGW --> Web
    Web --> App
    App --> DB
    App -.-> S3
