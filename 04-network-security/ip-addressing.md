# Network Addressing Architecture

## Purpose

This document defines the IP addressing model and subnet segmentation used in the Enterprise Security Homelab.

The addressing strategy supports:

- Tier-based enforcement
- Predictable logging and SIEM correlation
- Infrastructure role clarity
- Future VLAN expansion

---

## Addressing Model

The environment uses RFC1918 private addressing.

Two primary networks are defined:

### Management Network (vmbr0)

- Subnet: 192.168.1.0/24
- Proxmox Management Interface: 192.168.1.8
- Firewall WAN Interface: 192.168.1.171

Purpose:

- Upstream internet access
- Hypervisor management
- Separation from internal enterprise traffic

---

### Internal Enterprise Network (vmbr1)

- Subnet: 192.168.10.0/24
- Firewall Gateway: 192.168.10.1

Purpose:

- Hosts all domain infrastructure
- Enforces tier-based segmentation
- Centralized logging and monitoring

---

## Static Addressing Strategy

All infrastructure systems use static IP assignments.

Reasons:

- Predictable SIEM correlation
- Simplified firewall rule design
- Clear infrastructure mapping
- Reduced DHCP dependency for critical systems

---

## Infrastructure Address Map

| IP Address       | Hostname          | Role |
|------------------|-------------------|------|
| 192.168.10.1     | OPNsense          | Firewall Gateway |
| 192.168.1.8      | PROXMOX-MGMT      | Hypervisor Management |
| 192.168.10.10    | DC01              | Tier 0 Domain Controller |
| 192.168.10.20    | PBS01             | Backup Infrastructure |
| 192.168.10.40    | SIEM01            | Wazuh SIEM |
| 192.168.10.60    | WIN11-Admin01     | Tier 1 Admin Workstation |
| 192.168.10.70    | KALI01            | Attack Simulation Host |
| 192.168.10.100   | WIN11-Client01    | Tier 2 Endpoint |

---

## DNS Model

Active Directory DNS is provided by DC01 (192.168.10.10).

All domain-joined systems use DC01 as primary DNS.

The firewall does not provide internal DNS services.

This mirrors enterprise identity-centric DNS design.

---

## Routing Model

OPNsense handles:

- Routing between management and internal networks
- Internet egress control
- East/West traffic filtering

No direct routing exists between networks outside firewall inspection.

---

## Design Considerations

The addressing layout:

- Aligns with Active Directory tiering
- Simplifies firewall alias usage
- Supports future VLAN separation
- Enables structured monitoring visibility

---

## Future Expansion

Planned enhancements:

- VLAN segmentation per tier
- Dedicated SIEM subnet
- Isolated malware analysis network
- Additional infrastructure segments