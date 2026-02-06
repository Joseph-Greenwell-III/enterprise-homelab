# Networking & Segmentation

## VLAN Design
| VLAN | Purpose |
|-----|--------|
| 10 | Management |
| 20 | Servers |
| 30 | Workstations |
| 99 | Isolated / Testing |

## Firewall Responsibilities
- OPNsense provides routing and DHCP
- Domain Controller provides DNS for AD

## Design Rationale
This mirrors common enterprise setups where perimeter devices handle routing while AD-integrated DNS supports authentication and service discovery.