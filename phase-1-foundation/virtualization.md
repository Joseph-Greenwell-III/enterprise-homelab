# Virtualization Platform Design

## Objective
This document describes the virtualization platform used to host the enterprise-homelab environment.  
The goal is to provide a stable, secure, and scalable foundation that supports segmentation, backup, and repeatable system deployment.

The virtualization layer is intentionally designed to resemble small-to-medium enterprise infrastructure rather than a flat or convenience-driven lab.

## Hypervisor Platform
- **Platform:** Proxmox Virtual Environment
- **Version:** 9.1.4
- **Role:** Central virtualization and management platform

Proxmox is used to host all infrastructure, security, and client systems in the lab.

## Virtual Machine Organization
Virtual machines are named and organized by function to improve clarity and manageability.

**Key virtual machines include:**
- OPNsense (Firewall)
- DC01 (Domain Controller)
- WIN11-Admin01 (Tier 1 Admin Workstation)
- WIN11-Client01 (Tier 2 Client)
- Wazuh Server (SIEM)
- Kali Linux (Attack Simulation)
- Proxmox Backup Server

This naming convention simplifies monitoring, logging, and administrative workflows.

## Network Integration
Proxmox virtual bridges are used to support network segmentation.

- **vmbr0**
  - Internal lab network
  - Connected to OPNsense LAN interface
- **vmbr1**
  - Home network
  - Connected to OPNsense WAN interface

No virtual machines bypass the firewall; all traffic is routed through OPNsense.

## Storage Architecture
Proxmox uses local storage for virtual machine disks with centralized backup handling.

- VM disks stored on Proxmox-managed storage
- Backup data stored on Proxmox Backup Server
- Cold storage backups represent offline or detached copies

Storage is designed to support recovery testing rather than high availability.

## Backup & Recovery Strategy
Backups are a core design consideration rather than an afterthought.

### Proxmox Backup Server
- Centralized backup target
- Supports scheduled backups
- Enables VM-level restoration and testing

### Cold Storage Backup
- Represents offline or detached backup media
- Protects against ransomware and destructive events
- Not continuously accessible from the lab network

## Golden Images
Golden images are maintained for repeatable deployment.

### Golden Admin Image
- Windows 11 administrative workstation baseline
- Hardened configuration
- Used to rapidly deploy Tier 1 admin systems

### Golden Client Image
- Windows 11 client baseline
- Represents standard user endpoint configuration
- Used for consistent testing and telemetry generation

Golden images reduce configuration drift and improve consistency across deployments.

## Resource Allocation Philosophy
Resources are allocated intentionally based on system role.

- Infrastructure systems receive stable allocations
- Client systems reflect realistic enterprise endpoints
- Attack systems are constrained to limit impact

This mirrors real-world prioritization rather than maximizing performance.

## Operational Benefits
This virtualization design provides:

- Strong isolation between systems
- Predictable network flows
- Easy system recovery
- Repeatable deployments
- A stable platform for security experimentation

## Future Enhancements
Planned virtualization improvements include:
- Snapshot-based testing workflows
- Automation for VM provisioning
- Resource monitoring integration
- Infrastructure-as-code experiments
