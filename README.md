> ⚠️ **Notice**
>
> This repository represents a simulated enterprise-style homelab.
>
> All systems, domain names, IP addresses, and credentials are fictional or internal-only.
>
> Issues are used for tracking future improvements and phase planning.

# Enterprise Security Homelab

This repository documents the design, deployment, and evolution of an enterprise-style homelab environment built to develop hands-on cybersecurity and SOC analyst skills.

## Objectives
- Simulate a small enterprise Active Directory environment
- Practice Windows administration, networking, and security monitoring
- Build a foundation for SIEM, detection engineering, and incident response

## Current Status
✅ Phase 1 – Core Infrastructure (Completed)  
⏳ Phase 2 – Centralized Logging (Planned)  
⏳ Phase 3 – Attack & Detection (Planned)

## Environment Overview
- Hypervisor: Proxmox
- Firewall: OPNsense
- Directory Services: Windows Server Active Directory
- Clients: Windows 11, Linux
- Network Segmentation via VLANs

## Architecture
![Network Diagram](phase-1-foundation/screenshots/architecture/network-diagram.png)

## Phases
- [Phase 1 – Core Infrastructure](phase-1-foundation/overview.md)
- Phase 2 – SIEM & Log Aggregation (Planned)
- Phase 3 – Adversary Simulation & Detection (Planned)

## Future Enhancements
- Windows Event Forwarding
- Splunk SIEM
- Sysmon telemetry
- Detection engineering aligned to MITRE ATT&CK

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Attribution

This repository reflects my personal design decisions, implementation choices, and documentation approach for an enterprise-style homelab project.  
While others are welcome to learn from or reference this work in accordance with the license, this repository represents original effort and ongoing iteration.