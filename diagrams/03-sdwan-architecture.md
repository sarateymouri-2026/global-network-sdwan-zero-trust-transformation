# Secure SD-WAN Fabric & Application-Aware Routing

```mermaid
flowchart TB
    Orchestrator["SD-WAN Central Orchestrator & Controller<br/>(Zero-Touch Provisioning)"]

    subgraph Underlay ["Hybrid Transport Underlay"]
        Fiber["Primary DIA Fiber Link"]
        Cell["Secondary 5G / Broadband Link"]
    end

    subgraph EdgeDevice ["Branch Edge Appliance"]
        AAR["Application-Aware Routing<br/>(Dynamic Path Selection & IPsec Encrypted Overlay)"]
    end

    LAN["Branch LAN Endpoints<br/>(Users, VoIP, IoT Segments)"]

    Orchestrator -->|"Push Policies"| Fiber
    Orchestrator -->|"Push Policies"| Cell
    Fiber -->|"SLA Monitoring"| AAR
    Cell -->|"SLA Monitoring"| AAR
    AAR --> LAN
