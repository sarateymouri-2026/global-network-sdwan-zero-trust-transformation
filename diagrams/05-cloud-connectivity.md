# Multi-Cloud Connectivity & Transit Gateway Architecture

```mermaid
flowchart TB
    Gateway["Global Cloud SASE / SD-WAN Gateway"]

    subgraph AWS ["AWS Cloud VPC"]
        TGW["AWS Transit Gateway"]
        AppAWS["Production Workloads"]
        TGW --> AppAWS
    end

    subgraph Azure ["Azure Cloud VNet"]
        Hub["Azure Virtual Hub"]
        AppAzure["Analytics & DR Workloads"]
        Hub --> AppAzure
    end

    Gateway ==>|"Encrypted IPsec Tunnel"| TGW
    Gateway ==>|"Encrypted IPsec Tunnel"| Hub
