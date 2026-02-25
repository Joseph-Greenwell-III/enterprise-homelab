# Enterprise Security Homelab

<p align="center">
  <a href="00-overview/architecture-diagram.png">
    <img src="00-overview/architecture-diagram.png" alt="Enterprise Security Homelab Architecture Diagram" width="900">
  </a>
</p>

## Architectural Highlights

- Centralized firewall enforcement (no workload bypass)
- Tiered Active Directory administrative model (Tier 0 / 1 / 2)
- Dedicated privileged access workstations (PAWs)
- Default-deny network posture
- Isolated monitoring plane (Wazuh SIEM)
- Controlled adversary simulation environment
- Structured recovery and change management practices

---

## Overview

This repository documents the architecture, segmentation strategy, and security controls of a tiered Active Directory environment built on Proxmox with OPNsense firewall enforcement and centralized Wazuh SIEM monitoring.

The environment models a small-to-mid-sized enterprise network with administrative tier separation, explicit trust boundaries, and centralized logging. It serves as the foundational platform for adversary simulation and detection engineering documented separately in the Cybersecurity Projects repository.

This lab is intentionally engineered to reflect real-world enterprise security architecture rather than a flat or convenience-driven test environment.

---

## Architecture Components

### Virtualization

- Proxmox Virtual Environment 9.1.4
- Segmented virtual bridges:
  - `vmbr0` — 192.168.1.0/24 (Management / Home LAN)
  - `vmbr1` — 192.168.10.0/24 (Internal Enterprise Network)
- Hypervisor isolated from domain authentication
- All inter-network traffic traverses OPNsense

### Network Security

- OPNsense firewall acting as Policy Enforcement Point (PEP)
- Alias-based rule abstraction for clarity and maintainability
- Explicit east-west traffic restrictions
- DNS and management access scoped by policy
- Default posture: deny-by-design

### Identity & Directory Services

- Windows Server Active Directory (`corp.lab`)
- Tiered administrative model:
  - **Tier 0** — Domain Controllers and identity infrastructure
  - **Tier 1** — Member servers and administrative systems
  - **Tier 2** — User endpoints
- Dedicated Privileged Access Workstations (PAWs)
- Scoped Group Policy per tier and role
- Microsoft Security Baselines implemented and customized

### Monitoring & Logging

- Wazuh SIEM deployed in a controlled management segment
- Centralized telemetry ingestion via explicit firewall rules
- Logging policies deployed through GPO
- Host-level protections on monitoring infrastructure

---

## Security Architecture Summary

### 1. Administrative Tier Separation

Administrative access is restricted to its respective tier to reduce credential exposure and limit privilege escalation paths.

Tier 0 access is performed only from dedicated privileged workstations. Credentials are not reused across tiers.

This mirrors enterprise identity hardening strategies designed to mitigate lateral movement and credential theft.

---

### 2. Explicit Network Segmentation

The lab is intentionally segmented:

- Management Network (Home LAN)
- Internal Enterprise Network
- Isolated Adversary Simulation Zone

OPNsense enforces:

- Controlled DNS access to the Domain Controller
- Restricted administrative paths
- Telemetry-only rules to the SIEM
- Explicit block rules preventing unnecessary lateral movement

There is no implicit trust between segments. Traffic is permitted only through explicit policy.

---

### 3. Scoped Group Policy Strategy

Group Policy Objects are separated by:

- Tier
- System role (Workstation / Server / PAW)
- Function (Logging, Defender, BitLocker, Credential Guard, etc.)

Security baselines are applied in a controlled and scoped manner rather than domain-wide without review.

This ensures:

- Clear change boundaries  
- Reduced blast radius of misconfiguration  
- Easier troubleshooting and recovery  

---

### 4. Centralized Logging & Detection Foundation

The Wazuh SIEM is deployed in a restricted management segment with:

- Explicit firewall ingestion rules
- Controlled port exposure
- Hardened host configuration

Logging policies enable:

- Security event auditing
- Endpoint visibility
- Measurable detection validation during adversary simulation

The architecture supports structured attack simulation and containment analysis.

---

### 5. Assume-Breach Security Model

The environment includes a dedicated adversary simulation host to model:

- Credential theft
- Lateral movement attempts
- Privilege escalation paths
- Detection coverage validation

Security controls are implemented under the assumption that initial compromise is possible. The objective is containment, visibility, and recovery—not artificial perfection.

---

## Repository Scope

This repository documents the enterprise platform itself:

- Virtualization architecture
- Identity design
- Network security enforcement
- Endpoint hardening
- Logging and monitoring architecture
- Resilience and recovery procedures
- Change management practices

Adversary simulations, detection engineering writeups, and incident case studies are maintained separately to preserve architectural clarity.

---

## Design Philosophy

This lab prioritizes:

- Least privilege
- Explicit trust boundaries
- Administrative isolation
- Centralized enforcement
- Change control discipline
- Recoverability after misconfiguration
- Realistic enterprise modeling over convenience

The platform is iterative and designed for structured expansion, including future VLAN segmentation, additional telemetry sources, and enhanced detection engineering capabilities.

---

## License

MIT License. See LICENSE file for details.