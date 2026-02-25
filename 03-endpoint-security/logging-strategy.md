# Logging Strategy

## Objective

Provide centralized visibility across all tiers to support detection engineering and adversary simulation.

Logging is designed to measure containment and policy effectiveness.

---

## Deployment Model

Separate logging GPOs are deployed per tier.

These enable:

- Security event auditing
- Account logon tracking
- Privilege use monitoring
- PowerShell logging (if configured)
- Windows Defender logging

Logging configuration is scoped per OU.

---

## SIEM Integration

Wazuh agents are deployed to:

- Tier 1 systems
- Tier 2 systems
- Infrastructure servers

Firewall rules explicitly permit telemetry traffic to the SIEM.

There is no unrestricted inbound logging traffic.

---

## Network Controls

Logging traffic:

- Traverses the firewall
- Is restricted to defined ports
- Is limited to the SIEM host

This ensures that monitoring infrastructure does not expand the attack surface.

---

## Future Enhancements

Planned improvements:

- Windows Event Forwarding (optional)
- Enhanced PowerShell transcript logging
- Sysmon deployment
- Detection rule tuning based on adversary simulation

Logging strategy evolves as detection maturity increases.