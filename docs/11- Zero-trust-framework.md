# Zero Trust Architecture (ZTA) & Microsegmentation Framework

## 1. Core Architectural Tenets (NIST SP 800-207 Aligned)
This architecture eliminates implicit trust based on physical or network location (e.g., being inside a branch LAN or connected via legacy IPsec VPN). Every session is evaluated dynamically before access is granted:

1. **Continuous Verification:** Authentication and authorization are evaluated per-session using user identity (IdP/SSO), device health posture (EDR/MDM compliance), and behavioral risk scores.
2. **Least-Privilege Access:** Users and workloads are granted access strictly to specific application endpoints (Layer 7) rather than entire IP subnets (Layer 3).
3. **Assume Breach:** Network segments are isolated by default so that a compromised endpoint in a branch office cannot pivot laterally to corporate data centers or cloud VPCs.

---

## 2. Macro- vs. Micro-Segmentation Design

| Segmentation Layer | Enforcement Point | Mechanism | Purpose |
| :--- | :--- | :--- | :--- |
| **WAN Macro-Segmentation** | SD-WAN Edge Routers | VRF / VPN Overlay Separation | Separates Corporate, Guest, IoT/OT, and PCI/Compliance traffic across the global fabric. |
| **Branch Micro-Segmentation** | Campus Switches & Edge Firewalls | 802.1X, SGTs (Security Group Tags), Dynamic VLANs | Prevents workstation-to-workstation and IoT-to-corporate communication within the same site. |
| **Cloud & Data Center Micro-Segmentation** | Cloud-Native Firewalls & Host Agents | Identity-based Security Groups & eBPF policies | Enforces east-west traffic inspection between microservices and database tiers. |

---

## 3. Dynamic Trust Evaluation Workflow

```mermaid
sequenceDiagram
    autonumber
    participant User as User / Endpoint
    participant Edge as SD-WAN / SASE Edge
    participant IdP as Identity Provider (SSO/MFA)
    participant Posture as MDM / EDR Posture Engine
    participant App as Target Application

    User->>Edge: Request access to internal ERP
    Edge->>IdP: Validate SAML/OIDC token & MFA state
    IdP-->>Edge: Identity Verified (Role: Finance)
    Edge->>Posture: Query device compliance & risk score
    Posture-->>Edge: Device Compliant (OS Patched, EDR Active)
    Edge->>Edge: Evaluate ZTNA Policy (Rule ZTNA-FIN-004)
    Edge->>App: Establish encrypted micro-tunnel (mTLS)
    App-->>User: Session Granted (Continuous monitoring active)
