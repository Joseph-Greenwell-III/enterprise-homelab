# Virtualization Architecture

## 1. Architectural Purpose

The virtualization layer provides the foundational compute and network
substrate for the Enterprise Security Homelab. It is designed to model a
small-to-medium enterprise deployment where management, segmentation,
and security enforcement are intentionally structured rather than
convenience-driven.

This layer supports:

-   Segmented virtual networking
-   Centralized firewall enforcement
-   Administrative boundary separation
-   Backup and recovery operations
-   Controlled system deployment and testing

The hypervisor is treated as critical infrastructure and protected
accordingly.

------------------------------------------------------------------------

## 2. Hypervisor Platform

-   **Platform:** Proxmox Virtual Environment\
-   **Version:** 9.1.4\
-   **Role:** Central virtualization and management platform

Proxmox hosts all infrastructure, security, and endpoint virtual
machines within the lab environment.

All systems---including identity infrastructure, SIEM, client systems,
and adversary simulation hosts---are virtualized and governed by this
control plane.

------------------------------------------------------------------------

## 3. Hypervisor Trust Boundary

The Proxmox host is treated as a management-plane asset and is **not
domain-joined**.

Administrative access to the hypervisor:

-   Resides exclusively on the management network (vmbr0 --
    192.168.1.0/24)
-   Is not directly accessible from internal lab systems without
    firewall traversal
-   Is logically separated from domain authentication mechanisms

This separation ensures that compromise of a domain-joined workload does
not directly expose the virtualization control plane.

The hypervisor represents a high-value asset and is therefore isolated
from the enterprise identity boundary.

------------------------------------------------------------------------

## 4. Virtual Networking Design

Proxmox virtual bridges enforce separation between management access and
internal lab traffic.

### vmbr0 -- Management Network

-   Subnet: 192.168.1.0/24
-   Hosts Proxmox management interface
-   Provides upstream connectivity
-   Considered external relative to the internal lab network

### vmbr1 -- Internal Enterprise Network

-   Subnet: 192.168.10.0/24
-   Connected to the OPNsense LAN interface
-   Hosts all domain-joined infrastructure and endpoints
-   Does not have upstream connectivity without traversing OPNsense for
    NAT and firewall inspection

No virtual machines bypass the firewall. All inter-network traffic
traverses OPNsense, which acts as the Policy Enforcement Point (PEP).

------------------------------------------------------------------------

## 5. Virtual Machine Organization

Virtual machines are named and organized by functional role to improve
operational clarity and log correlation.

Core systems include:

-   OPNsense (Firewall / PEP)
-   DC01 (Tier 0 Domain Controller & AD DNS)
-   WIN11-Admin01 (Tier 1 Administrative Workstation)
-   WIN11-Client01 (Tier 2 Client Endpoint)
-   SIEM01 (Wazuh Monitoring Platform)
-   KALI01 (Adversary Simulation Host)
-   PBS01 (Proxmox Backup Server)

Naming conventions reflect system purpose rather than arbitrary
numbering, supporting clearer detection engineering and monitoring
workflows.

------------------------------------------------------------------------

## 6. Backup Integration

Proxmox Backup Server (PBS01) provides VM-level backup capabilities for
resilience testing and recovery operations.

Backup strategy characteristics:

-   Centralized VM snapshot backups
-   Role-based protection of critical infrastructure (e.g., Domain
    Controller)
-   Separation between production workloads and backup infrastructure
-   Offline / cold storage integration for recovery validation

The virtualization layer supports controlled failure and restoration
scenarios to model real-world operational resilience.

------------------------------------------------------------------------

## 7. Security Considerations

The virtualization architecture enforces several core security
principles:

-   Hypervisor management is isolated from internal workloads
-   Internal lab systems cannot directly access the management plane
-   All east/west and north/south traffic traverses the firewall
-   Static addressing improves monitoring and log correlation
-   Infrastructure systems are intentionally segmented by role

Default posture within the lab environment is deny-by-design, with
traffic explicitly permitted through defined policy.

------------------------------------------------------------------------

## 8. Architectural Evidence

The following screenshot documents the Proxmox network bridge
configuration used to enforce segmentation between management and
internal lab traffic.

![Proxmox Network
Bridges](screenshots/virtualization/proxmox-network-bridges.png)

This configuration demonstrates:

-   vmbr0 bound to the physical management interface
-   vmbr1 dedicated to internal lab workloads
-   Clear separation between hypervisor access and enterprise traffic

------------------------------------------------------------------------

## 9. Future Evolution

Planned enhancements to the virtualization layer include:

-   VLAN tagging for tier-based segmentation
-   Dedicated monitoring subnet
-   Isolated malware analysis segment
-   Additional adversary simulation environments

All evolution will preserve the principle that no workload bypasses
centralized firewall enforcement or management-plane isolation.
