# IP Addressing & VLAN Plan

## Site 1 — Josh's Home (10.10.0.0/16)

| VLAN ID | Name | Subnet | Gateway | DHCP Range | Notes |
|---|---|---|---|---|---|
| 10 | Users | 10.10.10.0/24 | 10.10.10.1 | .100-.200 | Workstations, laptops |
| 20 | Servers | 10.10.20.0/24 | 10.10.20.1 | Static only | AD, Docker, File Server |
| 30 | Management | 10.10.30.0/24 | 10.10.30.1 | Static only | pfSense mgmt, switch mgmt |
| 40 | Guest | 10.10.40.0/24 | 10.10.40.1 | .100-.200 | Isolated, internet-only |

## Site 2 — Drake's Home (10.20.0.0/16)

| VLAN ID | Name | Subnet | Gateway | DHCP Range | Notes |
|---|---|---|---|---|---|
| 10 | Users | 10.20.10.0/24 | 10.20.10.1 | .100-.200 | Workstations, laptops |
| 20 | Servers | 10.20.20.0/24 | 10.20.20.1 | Static only | Monitoring, Wazuh, Web |
| 30 | Management | 10.20.30.0/24 | 10.20.30.1 | Static only | pfSense mgmt, switch mgmt |
| 40 | Guest | 10.20.40.0/24 | 10.20.40.1 | .100-.200 | Isolated, internet-only |

## VPN Transit Network
| Network | Purpose |
|---|---|
| 10.99.0.0/30 | Site-to-site WireGuard tunnel (Site1 = .1, Site2 = .2) |

## Static IP Assignments (fill in as devices are built)

### Site 1
| Hostname | VLAN | IP | Purpose |
|---|---|---|---|
| pfsense-s1 | 30 | 10.10.30.1 | Firewall/router |
| | | | |

### Site 2
| Hostname | VLAN | IP | Purpose |
|---|---|---|---|
| pfsense-s2 | 30 | 10.20.30.1 | Firewall/router |
| | | | |

## Firewall Rule Matrix (baseline — refine before Phase 1 testing)

| Source VLAN | Dest VLAN | Allowed? | Notes |
|---|---|---|---|
| Users → Servers | | Allow (specific ports only) | Define per-service later |
| Users → Mgmt | | Deny | No exceptions |
| Guest → Servers | | Deny | |
| Guest → Mgmt | | Deny | |
| Guest → Users | | Deny | |
| Mgmt → Any | | Allow | Admin source only |
| Servers → Mgmt | | Deny (unless specific need) | |
| Site1 ↔ Site2 (VPN) | | Allow (defined ports only) | Tighten once services are known |
