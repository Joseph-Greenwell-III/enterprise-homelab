# IP Addressing Scheme

## Overview
This document describes the IP addressing and network segmentation used in the enterprise-homelab environment.  
The design separates the home LAN from the internal lab network while allowing controlled routing through a dedicated firewall.

The goal is to mirror enterprise network design principles that support security monitoring, identity infrastructure, and controlled administrative access.

## Platform Context
- Hypervisor: Proxmox Virtual Environment 9.1.4
- Firewall: OPNsense
- Virtual bridges:
  - vmbr0 – Home LAN
  - vmbr1 – Internal lab network

## Network Segmentation

### Home LAN (vmbr0)
- Subnet: `192.168.1.0/24`
- Firewall WAN IP: `192.168.1.171`
- Purpose:
  - Provides upstream internet access
  - Isolates the lab from the physical home network
  - Prevents lateral movement from lab systems into the home LAN

### Internal Lab Network (vmbr1)
- Subnet: `192.168.10.0/24`
- Firewall IP (Gateway): `192.168.10.1`
- Purpose:
  - Hosts all enterprise lab infrastructure
  - Supports Active Directory, SIEM, backup, and client systems
  - Enforces east/west traffic controls via firewall rules

## Addressing Strategy
- RFC1918 private addressing
- Single internal /24 subnet for Phase 1 simplicity
- Static IP assignments for all infrastructure systems
- Addressing aligned to system role rather than VM ID

This structure allows easy expansion into additional VLANs or subnets in later phases (e.g., SIEM, admin workstations, attack simulation).

## Static IP Assignments

| IP Address       | Hostname          | Role / Purpose |
|------------------|-------------------|----------------|
| 192.168.10.1     | OPNsense          | Lab network gateway |
| 192.168.1.8      | PROXMOX-MGMT      | Proxmox VE management interface (Home LAN) |
| 192.168.10.10    | DC01              | Tier 0 Active Directory Domain Controller & AD DNS |
| 192.168.10.20    | PBS01             | Proxmox Backup Server |
| 192.168.10.40    | SIEM01            | Wazuh SIEM server |
| 192.168.10.60    | WIN11-Admin01     | Tier 1 administrative workstation |
| 192.168.10.70    | KALI01            | Attack simulation / testing |
| 192.168.10.100   | WIN11-Client01    | Tier 2 client workstation |

## DNS and Directory Services
- Active Directory DNS is provided by DC01 (`192.168.10.10`)
- All domain-joined systems use DC01 as their primary DNS server
- Firewall is not used for DNS resolution within the lab

This mirrors enterprise environments where identity infrastructure owns internal name resolution.

## Firewall and Routing Model
- OPNsense handles:
  - Routing between home LAN and lab network
  - Internet egress control
  - East/west traffic filtering within the lab
- Explicit firewall rules restrict:
  - Lab → Home LAN traffic
  - Lateral movement between client systems
  - Administrative access to Tier 0 assets

Network aliases are used extensively to simplify rule management and improve readability.

## Design Considerations
- Static addressing ensures predictable logging and correlation in the SIEM
- Centralized gateway simplifies traffic inspection and control
- Tier-based system placement supports Active Directory security model
- Addressing layout aligns with future Active Directory Sites and Services mapping

## Future Expansion
Planned enhancements include:
- Dedicated VLANs for Tier 0, Tier 1, and Tier 2 systems
- Separate subnet for SIEM and logging infrastructure
- Isolated network segments for malware analysis and red team testing
