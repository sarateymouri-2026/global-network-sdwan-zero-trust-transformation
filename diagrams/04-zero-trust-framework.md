### 4. `diagrams/04-zero-trust-framework.md`
```markdown
# Zero Trust Network Access (ZTNA) Verification Workflow

```mermaid
flowchart LR
    User["User & Device EDR"] -->|"1. Authenticate"| IdP["Identity Provider<br/>(MFA / SSO)"]
    User -->|"2. Posture Check"| MDM["MDM / EDR Compliance<br/>(OS Patched)"]
    
    IdP -->|"3. Token Valid"| PDP["ZTNA Policy Engine<br/>(Contextual Evaluation)"]
    MDM -->|"4. Compliant"| PDP
    
    PDP -->|"5. Grant Secure mTLS Session"| App["Protected Enterprise App<br/>(Microsegmented)"]
