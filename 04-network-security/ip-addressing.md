# IP Addressing Strategy

## Overview

The IP addressing scheme within the enterprise homelab is designed to
support security segmentation aligned with a tiered administrative
model. Network addressing is organized based on trust level and
operational function rather than purely device classification.

## Network Ranges

  -----------------------------------------------------------------------
  Network                    CIDR              Purpose
  -------------------------- ----------------- --------------------------
  Home LAN (vmbr0)           192.168.1.0/24    Management / External
                                               Network

  Internal Lab Network       192.168.10.0/24   Enterprise Lab Environment
  (vmbr1)                                      
  -----------------------------------------------------------------------

## Routing Model

Traffic between management and internal lab networks traverses the
OPNsense firewall, which acts as the central Policy Enforcement Point
(PEP).

## Security Segmentation Alignment

The IP addressing model directly supports the Active Directory tiered
administration model and firewall enforcement strategy.

Infrastructure systems are grouped logically by security trust level
rather than organizational function.

  Security Tier          Address Range                     Purpose
  ---------------------- --------------------------------- -------------------------
  Tier 0                 192.168.10.0 -- 192.168.10.49     Identity Infrastructure
  Tier 1                 192.168.10.50 -- 192.168.10.89    Administrative Systems
  Tier 2                 192.168.10.90 -- 192.168.10.149   User Endpoints
  Adversary Simulation   192.168.10.150+                   Attack Testing

This structured allocation enables: - Firewall alias abstraction -
Tier-aware traffic enforcement - SIEM correlation by subnet - Rapid
incident containment

## Design Objectives

-   Support privilege separation
-   Enable deterministic firewall policy creation
-   Simplify incident response analysis
-   Provide scalable enterprise-style segmentation
