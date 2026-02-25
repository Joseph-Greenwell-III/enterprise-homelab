# Architecture Overview

## 1. Architectural Intent

The Enterprise Homelab is designed to model a segmented,
security-focused enterprise network that enforces clear trust
boundaries, administrative tier separation, and centralized monitoring.

The architecture prioritizes:

-   Explicit traffic enforcement
-   Identity protection through tiering
-   Controlled management access
-   Centralized telemetry collection
-   Resilience and recoverability

This environment is structured as a long-term security engineering
platform rather than a convenience-based lab.

------------------------------------------------------------------------

## 2. Network Topology

The environment is built on Proxmox Virtual Environment 9.1.4 using two
primary virtual bridges:

### Management Network -- vmbr0 (192.168.1.0/24)

-   Hosts the Proxmox management interface
-   Provides upstream internet connectivity
-   Considered external relative to the internal lab network
-   Does not provide direct access to internal systems without firewall
    traversal

### Internal Enterprise Network -- vmbr1 (192.168.10.0/24)

-   Hosts all domain-joined infrastructure and client systems
-   Routed and enforced by OPNsense
-   Subject to explicit firewall policy and default-deny posture

All traffic between segments traverses the OPNsense firewall. No system
bypasses centralized enforcement.

------------------------------------------------------------------------

## 3. Trust Boundaries

The architecture defines multiple explicit trust boundaries:

### External Boundary

-   Home LAN vs. Internal Lab Network
-   Enforced by OPNsense routing and firewall policy

### Identity Boundary

-   Tier 0 (Domain Controllers and privileged accounts)
-   Tier 1 (Administrative systems and servers)
-   Tier 2 (User workstations)

Administrative credentials are not reused across tiers. Tier 0 access is
restricted to dedicated privileged workstations.

### Monitoring Boundary

-   Wazuh SIEM resides in a controlled management segment
-   Telemetry flows are restricted and documented
-   SIEM-initiated communication is limited to reduce attack surface

------------------------------------------------------------------------

## 4. Identity Control Plane

The identity infrastructure centers around the corp.lab Active Directory
domain.

Core components include:

-   DC01 (Domain Controller and AD-integrated DNS)
-   Tiered Organizational Unit (OU) structure
-   Scoped Group Policy deployment
-   Microsoft Security Baselines applied per tier

Active Directory functions as the authentication authority and internal
DNS provider for all domain-joined systems.

The identity control plane is treated as Tier 0 and protected
accordingly.

------------------------------------------------------------------------

## 5. Management Plane

Administrative operations are intentionally constrained.

-   Proxmox management resides on the management network (vmbr0)
-   Tier 1 administrative workstations are used for domain and server
    management
-   Firewall rules restrict administrative access paths
-   No direct client-to-infrastructure administrative access is
    permitted

Management access is explicit and governed by policy rather than
convenience.

------------------------------------------------------------------------

## 6. Security Monitoring Plane

The environment includes centralized logging via a Wazuh SIEM
deployment.

Design characteristics:

-   Isolated SIEM host
-   Controlled inbound telemetry rules
-   Logging enabled through scoped Group Policy
-   Static IP addressing to support consistent correlation

This structure supports detection engineering, adversary simulation
validation, and incident response testing.

------------------------------------------------------------------------

## 7. Resilience and Recovery Layer

Resilience is incorporated into the architectural model.

-   Proxmox Backup Server provides VM-level backups
-   Cold storage concept supports offline recovery scenarios
-   Administrative tier separation reduces blast radius
-   Recovery documentation is maintained in the resilience section of
    the repository

The environment is designed to support controlled failure and recovery
testing.

------------------------------------------------------------------------

## 8. Architectural Diagram

The accompanying diagram visually represents:

-   Network segmentation (vmbr0 and vmbr1)
-   OPNsense as the central enforcement point
-   Tier 0 / Tier 1 / Tier 2 placement
-   SIEM isolation
-   Backup and cold storage concepts
-   Attack simulation zone

The diagram reflects logical architecture and trust boundaries rather
than physical topology.

------------------------------------------------------------------------

## 9. Evolution Strategy

The architecture is designed for structured expansion. Future
enhancements may include:

-   Dedicated VLANs per administrative tier
-   Additional telemetry sources
-   IDS/IPS integration
-   Hybrid identity integration
-   Expanded adversary simulation networks

All evolution aligns with the documented design principles to maintain
architectural integrity.
