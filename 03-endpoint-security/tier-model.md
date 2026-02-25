# Endpoint Tier Model

## Purpose

The endpoint tier model enforces administrative separation to reduce credential exposure and prevent privilege escalation across trust boundaries.

Endpoints are segmented according to their relationship to identity infrastructure and administrative authority.

This model aligns with enterprise Active Directory tiering principles.

---

## Tier Definitions

### Tier 0 – Identity Infrastructure

Tier 0 systems host or directly manage identity services.

Examples:

- Domain Controllers
- Identity management infrastructure
- Tier 0 Privileged Access Workstation (PAW)

Characteristics:

- Highest trust boundary
- Restricted interactive logon rights
- Administrative access performed only from Tier 0 PAWs
- No cross-tier credential usage

---

### Tier 1 – Administrative Systems

Tier 1 systems manage enterprise infrastructure but do not host identity services.

Examples:

- Member servers
- Proxmox Backup Server
- Wazuh Server
- Tier 1 Administrative Workstation

Characteristics:

- Managed using Tier 1 admin accounts
- Cannot authenticate to Tier 0 systems unless explicitly required
- Restricted from Tier 2 credential exposure

---

### Tier 2 – User Endpoints

Tier 2 systems represent standard user workstations.

Examples:

- Domain-joined client systems
- User productivity machines

Characteristics:

- No administrative privileges over higher tiers
- Restricted network access to infrastructure systems
- Subject to baseline hardening policies

---

## Enforcement Mechanisms

The tier model is enforced through:

- OU-based separation
- Scoped Group Policy deployment
- Dedicated administrative accounts per tier
- Firewall-enforced network segmentation
- Restricted logon rights via GPO

Administrative credentials are not reused across tiers.