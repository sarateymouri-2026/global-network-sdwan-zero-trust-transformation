# Current State: Legacy Hub-and-Spoke Network Architecture

```mermaid
flowchart TB
    subgraph BranchA [Branch Office A]
        BA_User[Users / Devices]
    end
    
    subgraph BranchB [Branch Office B]
        BB_User[Users / Devices]
    end

    subgraph DC [Central Data Center - HQ Bottleneck]
        MPLS[Core MPLS Router] 
        FW[Legacy Perimeter Firewall\n(Inspection Bottleneck)]
        Internal[Internal Apps & Databases\n(Implicit Trust Zone)]
        
        MPLS --> FW --> Internal
    end

    Cloud[Cloud SaaS\n(M365 / Salesforce)]

    BA_User -->|Expensive MPLS Backhaul| MPLS
    BB_User -->|Expensive MPLS Backhaul| MPLS
    BA_User -.->|Forced / Suboptimal Access| Cloud
    BB_User -.->|Forced / Suboptimal Access| Cloud
