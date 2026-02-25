# Active Directory Architecture & Tiered Administration

## Purpose

This document defines the Active Directory identity architecture implemented within the Enterprise Security Homelab.

The design enforces administrative tier separation, reduces credential exposure, and aligns with enterprise Active Directory security practices focused on containment and least privilege.

---

## Domain Overview

- **Domain Name:** corp.lab  
- **Domain Controller:** DC01  
- **Roles:**
  - Active Directory Domain Services (AD DS)
  - AD-integrated DNS
  - Group Policy Management

DC01 represents the Tier 0 identity boundary and is treated as the highest trust asset in the environment.

The architecture supports future multi-domain-controller redundancy, though a single DC is deployed for current lab scope.

---

## Tiered Administration Model

The environment implements a three-tier administrative model to separate identity control, infrastructure management, and user systems.

---

### Tier 0 – Identity & Control Plane

Tier 0 contains the most sensitive identity systems and administrative accounts.

**Includes:**

- Domain Controllers
- AD-integrated DNS
- Group Policy infrastructure
- Tier 0 administrative accounts
- Tier 0 Privileged Access Workstation (PAW)

**Controls:**

- Only Tier 0 administrative accounts may access Tier 0 systems
- No internet access from Tier 0 systems
- Restricted administrative logon locations
- No cross-tier credential reuse

Compromise of Tier 0 equals domain compromise; therefore, strict containment is enforced.

---

### Tier 1 – Administrative Systems

Tier 1 contains systems and accounts used to manage enterprise infrastructure.

**Includes:**

- Administrative workstation (Tier 1)
- Server administration accounts
- Member servers
- Backup infrastructure
- Monitoring systems

**Controls:**

- Tier 1 accounts may administer Tier 1 systems
- Tier 1 systems may authenticate to Tier 0 services (e.g., AD) but do not manage identity infrastructure
- Tier 1 accounts are denied interactive logon to Tier 2 systems
- Separate credentials are used from Tier 0

This limits credential exposure across trust boundaries.

---

### Tier 2 – Client Systems

Tier 2 represents standard user systems and non-privileged accounts.

**Includes:**

- Domain-joined client workstations
- Standard user accounts

**Controls:**

- No administrative access to Tier 0 or Tier 1 systems
- Restricted lateral movement via firewall and policy
- Least privilege assignments enforced through GPO

---

## Organizational Unit (OU) Structure

The Active Directory OU hierarchy is organized by tier and function rather than by object type alone.

This structure enables:

- Scoped policy deployment
- Clear delegation boundaries
- Reduced blast radius of misconfiguration
- Tier-aligned administrative control

```text
corp.lab
├── Tier 0
│   ├── Tier 0 - Admin Groups
│   ├── Tier 0 - Admin Users
│   ├── Tier 0 - Servers
│   └── Tier 0 - Service Accounts
├── Tier 1
│   ├── Tier 1 - Admin Groups
│   ├── Tier 1 - Admin Servers
│   ├── Tier 1 - Admin Users
│   └── Tier 1 - Admin Workstations
├── Tier 2
│   ├── Tier 2 - Admin Groups
│   ├── Tier 2 - Admin Users
│   └── Tier 2 - Workstations
└── Users