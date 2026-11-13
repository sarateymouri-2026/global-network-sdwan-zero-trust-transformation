# Enterprise Security Policy & Access Matrix

```mermaid
flowchart TB
    subgraph Zones ["Network & Trust Zones"]
        Corp["Corporate Zone<br/>(Managed Users & Workstations)"]
        IoT["IoT & Guest Zone<br/>(Unmanaged / Isolated)"]
        Cloud["Cloud & SaaS Zone<br/>(Authorized Workloads)"]
    end

    subgraph Inspection ["SASE & Security Enforcement"]
        FW["Next-Gen Firewall / SWG<br/>(TLS Decryption & DLP)"]
        ZTNA["ZTNA Policy Engine<br/>(Contextual Least-Privilege)"]
    end

    Corp ==>|"Identity & Posture Check"| ZTNA
    IoT -.->|"Blocked Lateral Movement"| FW
    ZTNA -->|"Encrypted mTLS Session"| Cloud
    Corp -->|"Direct Secure Breakout"| Cloud
