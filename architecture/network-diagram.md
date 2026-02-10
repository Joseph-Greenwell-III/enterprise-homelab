# Network Architecture Diagram

This diagram represents the **intended network architecture** for the enterprise-style homelab.

It illustrates high-level design decisions, trust boundaries, and system roles that guide all implementation phases.

![Network Architecture Diagram](../phase-1-foundation/screenshots/architecture/network-diagram.png)

## Architectural Intent

The architecture is designed around the following principles:

- Clear separation between the home network and the internal lab environment
- A single enforcement point for all traffic via a dedicated firewall
- Tiered administrative boundaries (Tier 0 / Tier 1 / Tier 2)
- Controlled management access paths
- Support for attack simulation and monitoring
- Backup and recovery considerations, including offline storage

## Relationship to Implementation Phases

This architecture is **implemented incrementally** across project phases.

- **Phase 1 – Foundation**
  - Core network segmentation
  - Firewall enforcement
  - Active Directory tiering
  - Virtualization platform setup

Future phases may extend this architecture with additional controls, monitoring, and automation while preserving these core design principles.

For implementation details, screenshots, and configuration evidence, refer to:
- `phase-1-foundation/networking.md`
- `phase-1-foundation/virtualization.md`
