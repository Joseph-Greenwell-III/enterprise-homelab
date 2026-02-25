# Executive Summary

## Enterprise Security Homelab

This repository documents a segmented enterprise-style Active Directory
environment engineered to simulate real-world identity, network, and
security operations scenarios.

The environment is built to model enterprise-grade administrative
controls, network segmentation, centralized logging, and detection
engineering workflows. It serves as the foundational security platform
for adversary simulation and blue team exercises documented separately
in my Cybersecurity Projects repository.

------------------------------------------------------------------------

## Objective

The purpose of this environment is to:

-   Design and maintain a hardened Active Directory infrastructure
-   Implement enterprise-aligned administrative tiering (Tier 0 / 1 / 2)
-   Enforce network segmentation and explicit trust boundaries
-   Centralize telemetry for detection engineering
-   Support controlled adversary simulation and recovery testing

This lab prioritizes architectural discipline over convenience-based
configuration.

------------------------------------------------------------------------

## Environment Architecture

### Virtualization Layer

-   Proxmox Virtual Environment 9.1.4
-   Segmented virtual bridges for management and internal enterprise
    traffic

### Identity Architecture

-   Windows Server Active Directory domain: corp.lab
-   ESAE-inspired three-tier administrative model
-   Privileged Access Workstations (PAWs) for Tier 0 operations
-   Scoped Group Policy deployment strategy
-   Microsoft Security Baselines implemented and tailored

### Network Security

-   OPNsense firewall enforcing segmented traffic flow
-   Alias-based rule abstraction
-   Explicit lateral movement restrictions
-   Controlled management and telemetry access paths

### Monitoring & Detection Foundation

-   Isolated Wazuh SIEM deployment
-   Centralized event ingestion from domain systems
-   Logging policies deployed via GPO
-   Structured foundation for detection engineering and threat
    simulation

------------------------------------------------------------------------

## Security Philosophy

The environment is built around the following principles:

-   Least privilege access control
-   Administrative tier isolation
-   Explicit trust boundaries
-   Segmentation before detection
-   Assume-breach security modeling
-   Recoverability and change control discipline

The lab is intentionally designed to mirror enterprise operational
constraints and security practices rather than serve as a flat testing
network.

------------------------------------------------------------------------

## Operational Maturity

This repository documents:

-   Infrastructure architecture
-   Identity design decisions
-   Network enforcement controls
-   Logging and monitoring configuration
-   Recovery and resilience procedures
-   Iterative change tracking

Security simulations, adversary emulation, and detection case studies
are maintained separately to preserve architectural clarity.

------------------------------------------------------------------------

## Scope and Evolution

This environment is intentionally iterative. Future expansions may
include:

-   Extended telemetry sources
-   Advanced detection engineering workflows
-   Additional identity hardening controls
-   Hybrid or cloud-integrated identity components

All changes align with the documented design principles to maintain
architectural consistency and operational realism.
