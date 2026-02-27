# Firewall Architecture

## Overview

The OPNsense firewall serves as the central Policy Enforcement Point
(PEP) within the enterprise homelab. All inter-network traffic is
inspected and controlled according to a layered security policy aligned
with enterprise Zero Trust principles.

## Firewall Rule Philosophy

Firewall rules are designed according to security intent rather than
device-specific exceptions. Traffic is explicitly permitted only where
required for operational functionality.

## Policy Layered Enforcement Model

Firewall rules are organized into logical enforcement layers modeled
after enterprise network security architecture.

Rules are evaluated top-down according to functional purpose rather than
device type.

### 20 --- Identity Services

Permits authentication and directory operations required for domain
functionality.

Examples: - DNS - Kerberos - LDAP - NTP synchronization

Identity services are prioritized to ensure domain stability.

### 30 --- Management Plane

Restricts administrative management traffic to authorized privileged
systems.

Examples: - Administrative workstation → servers - Administrative
workstation → Domain Controller - Backup infrastructure communication

Administrative access paths are explicitly defined and monitored.

### 40 --- Telemetry and Logging

Ensures security telemetry reaches centralized monitoring systems before
isolation controls apply.

Examples: - Endpoint → Wazuh SIEM

Logging is preserved even during containment scenarios.

### 50 --- User Activity

Allows controlled outbound access required for endpoint operation.

Examples: - Internet browsing - System update services

User systems receive minimal outbound privileges.

### 80 --- Tier Isolation

Prevents privilege escalation across administrative tiers.

Examples: - Tier 2 → Tier 1 Admin blocked - Tier 2 → Tier 0 blocked -
Tier 1 → Tier 0 blocked - Server → Administrative workstation blocked

These controls enforce Microsoft's tiered administration model at Layer
3.

### 90 --- Containment Controls

Limits lateral movement and prevents compromise propagation.

Examples: - Endpoint-to-endpoint blocking - Lab-to-home network
isolation

Designed to simulate enterprise incident containment behavior.

### 99 --- Default Deny

All traffic not explicitly permitted is blocked.

## Privilege Boundary Enforcement

Network controls complement Active Directory permissions by enforcing
privilege separation independently of host security.

Even if credential compromise occurs, firewall isolation prevents
unauthorized administrative access paths.

This defense-in-depth model assumes endpoint compromise and limits
attacker progression.

## Security Validation

The firewall architecture enables practical validation through adversary
simulation.

Attack scenarios performed within the lab demonstrate:

-   Failed Tier 2 access to Tier 0 infrastructure
-   Blocked administrative pivot attempts
-   Contained lateral movement
-   Preserved SIEM telemetry during enforcement

This validates effective network-based privilege isolation.
