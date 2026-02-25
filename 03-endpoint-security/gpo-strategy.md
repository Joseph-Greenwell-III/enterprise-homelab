# Group Policy Strategy

## Design Principles

The Group Policy deployment model prioritizes:

- Scoped enforcement
- Minimal blast radius
- Tier-specific hardening
- Controlled change management

Baselines are not applied domain-wide.

---

## Baseline Model

Microsoft Security Baselines are deployed per tier:

- Tier 0 baseline GPO
- Tier 1 baseline GPO
- Tier 2 baseline GPO

Each baseline is uniquely named and linked only to its respective OU.

This ensures that:

- Identity infrastructure is hardened independently
- Administrative systems are scoped appropriately
- User endpoints receive least-privilege enforcement

---

## GPO Linking Strategy

GPOs are:

- Linked at the tier OU level
- Not enforced at the domain root
- Segmented by system role (Workstation / Server / PAW)
- Separated from logging-specific GPOs

This model reduces the blast radius of misconfiguration.

---

## Naming Convention

GPO naming reflects function and tier.

Examples:

- T0-DC-Baseline
- T1-W11-Baseline
- T2-Client-Baseline
- T1-Logging
- T2-Logging

Clear naming improves auditing and change control.

---

## Change Discipline

Policy modifications are:

- Scoped per tier
- Tested before enforcement
- Documented in change management records

This mirrors enterprise GPO governance practices.