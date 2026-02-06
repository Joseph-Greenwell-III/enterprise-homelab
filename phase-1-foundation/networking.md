\# Networking \& Traffic Control



\## Objective

This document describes the network security model used in the enterprise-homelab environment.  

The goal is to enforce clear trust boundaries, restrict lateral movement, and support centralized monitoring while maintaining functional administrative access.



\## Firewall Platform

\- \*\*Firewall:\*\* OPNsense

\- \*\*Role:\*\* Central routing and security enforcement point

\- \*\*Responsibilities:\*\*

&nbsp; - Routing between internal lab network and home network

&nbsp; - Internet egress control

&nbsp; - East/west traffic inspection

&nbsp; - Tier-based access enforcement



All inter-network communication traverses the firewall.



\## Network Zones



\### Home Network

\- Considered untrusted relative to the lab

\- No direct access into internal lab systems

\- Used only as an upstream internet connection



\### Internal Lab Network

\- Hosts all enterprise lab infrastructure

\- Protected by explicit allow rules and default deny behavior

\- Lateral movement is intentionally restricted



\## Firewall Rule Philosophy

Firewall rules are written according to the following principles:



\- \*\*Default Deny\*\*

&nbsp; - Traffic is blocked unless explicitly permitted

\- \*\*Least Privilege\*\*

&nbsp; - Rules allow only required ports, protocols, and source/destination pairs

\- \*\*Explicit Trust Boundaries\*\*

&nbsp; - Tier 0 systems are protected from lower-tier access

\- \*\*Readability \& Maintainability\*\*

&nbsp; - Network and service aliases are used extensively



\## Alias-Based Rule Design

To reduce rule complexity and improve clarity, aliases are used for:



\- \*\*Networks\*\*

&nbsp; - LAB\_NET

&nbsp; - HOME\_LAN

\- \*\*Hosts\*\*

&nbsp; - DC01

&nbsp; - WAZUH\_SIEM

&nbsp; - PBS\_SERVER

\- \*\*Port Groups\*\*

&nbsp; - AD\_TCP

&nbsp; - AD\_UDP

&nbsp; - WAZUH\_PORTS

&nbsp; - WEB\_PORTS



Aliases allow firewall rules to remain readable and scalable as the environment grows.



\## Key Traffic Flows



\### Identity Services

\- Clients and servers are permitted to communicate with \*\*DC01\*\* for:

&nbsp; - Authentication

&nbsp; - Directory services

&nbsp; - DNS resolution

\- Only required Active Directory ports are allowed



\### Administrative Access

\- \*\*Tier 1 administrative workstations\*\* are permitted access to:

&nbsp; - Tier 0 infrastructure (DC01)

&nbsp; - Management services (Proxmox, backups)

\- Administrative access from lower-tier systems is explicitly denied



\### Client Behavior

\- Tier 2 clients:

&nbsp; - Can authenticate to the domain

&nbsp; - Can reach the internet for updates

&nbsp; - Are restricted from lateral movement

&nbsp; - Cannot access administrative infrastructure



\### Security Monitoring

\- All systems are permitted to send telemetry to the \*\*Wazuh SIEM\*\*

\- Logging traffic is one-directional where possible

\- SIEM access back to clients is restricted



\### Backup Operations

\- Backup traffic is restricted to:

&nbsp; - Proxmox hosts

&nbsp; - Backup infrastructure

\- Backup systems do not initiate connections to clients



\### Attack Simulation

\- Kali Linux is isolated from sensitive infrastructure

\- Attack traffic is intentionally constrained

\- Used only for controlled testing and validation



\## Lateral Movement Controls

Firewall rules explicitly prevent:

\- Client-to-client communication

\- Client-to-administrative workstation access

\- Client-to-Tier 0 infrastructure access



This enforces a strong network-based control layer in addition to Active Directory permissions.



\## Internet Egress Controls

\- Only approved systems are allowed outbound internet access

\- Client internet access is restricted to required services

\- Infrastructure systems have tightly scoped outbound permissions



\## Security Benefits

This networking model provides:



\- Reduced attack surface

\- Clear blast-radius containment

\- Improved detection fidelity

\- Easier incident scoping

\- Enterprise-aligned security controls



\## Future Enhancements

Planned improvements include:

\- Additional VLANs for tier separation

\- IDS/IPS tuning

\- Egress filtering based on application identity

\- Enhanced logging of denied traffic



