# Global SD-WAN & Zero Trust Reference Topology

```mermaid
flowchart TB
    subgraph Branch["🏢 Global Branch Office (Zero-Touch Edge)"]
        Users["Corporate Users\n(Managed Endpoints)"]
        IoT["IoT / OT Devices\n(VRF 30 - Isolated)"]
        Edge["Secure SD-WAN Edge\n(DPI, Microsegmentation, IPsec)"]
        Users --> Edge
        IoT --> Edge
    end

    subgraph Remote["💻 Remote Workforce"]
        RemoteUser["Remote Employee\n(ZTNA Client + EDR)"]
    end

    subgraph Underlay["🌐 Hybrid Transport Underlay"]
        ISP1["Primary DIA Fiber"]
        ISP2["Secondary Broadband / 5G"]
    end

    subgraph SASE["🛡️ Cloud SASE & Zero Trust Policy Fabric"]
        IdP["Identity Provider\n(SSO / MFA / Device Posture)"]
        ZTNA["ZTNA Broker & SWG / FWaaS\n(Continuous Verification)"]
        IdP <--> ZTNA
    end

    subgraph Destinations["☁️ Cloud & Data Center Resources"]
        SaaS["Trusted SaaS\n(M365, Salesforce)"]
        MultiCloud["Multi-Cloud VPCs\n(AWS / Azure)"]
        DC["Core Data Center\n(ERP & Regulated DBs)"]
    end

    Edge ==>|Direct Breakout| SaaS
    Edge ==>|Encrypted Overlay| ISP1 & ISP2
    ISP1 & ISP2 ==> ZTNA
    RemoteUser ==>|mTLS Micro-Tunnel| ZTNA

    ZTNA -->|Least-Privilege Access| MultiCloud
    ZTNA -->|Least-Privilege Access| DC
