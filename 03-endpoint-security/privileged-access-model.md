# Privileged Access Model

## Objective

Reduce the risk of credential theft and lateral movement by isolating administrative actions to designated systems and tier boundaries.

This model assumes compromise is possible and focuses on containment.

---

## Account Separation

Separate administrative accounts exist per tier:

- Tier 0 Admin Accounts
- Tier 1 Admin Accounts
- Standard User Accounts

Rules:

- Tier 0 credentials are used only on Tier 0 systems.
- Tier 1 credentials are used only on Tier 1 systems.
- Standard user accounts never receive administrative privileges.

Credential reuse across tiers is prohibited.

---

## Privileged Access Workstations (PAWs)

Tier 0 administration is performed exclusively from dedicated PAWs.

PAW characteristics:

- Hardened using Microsoft Security Baselines
- No email, browsing, or general productivity use
- Restricted network access
- Limited interactive logon scope

---

## Tier 1 Administrative Workstation

A dedicated Tier 1 workstation is used to manage:

- Member servers
- Infrastructure services
- Security monitoring systems

This workstation:

- Uses Tier 1 administrative credentials
- Cannot authenticate to Tier 0 identity systems
- Is hardened via scoped baseline GPO

---

## Logon Restrictions

Logon controls are enforced using:

- Deny logon locally policies
- Deny logon through RDP policies
- OU-scoped GPO restrictions

These controls reduce cross-tier credential exposure and prevent privilege escalation chains.