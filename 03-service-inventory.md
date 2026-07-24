# Service Inventory

This document tracks every application and infrastructure service running in the CyberCorp Lab.

| Service | Host | Site | Purpose | Phase |
|----------|------|------|---------|-------|
| Active Directory Domain Services | DC-S1-01 | Site 1 | Authentication and authorization | 1 |
| DNS | DC-S1-01 | Site 1 | Internal name resolution | 1 |
| DHCP | pfSense | Both | Dynamic IP addressing | 1 |
| WireGuard | pfSense | Both | Site-to-site VPN | 1 |
| OSPF | pfSense | Both | Dynamic routing | 1 |
| SMB File Shares | FILE-S1-01 | Site 1 | Shared storage | 2 |
| Docker | DOCKER-S1-01 | Site 1 | Container platform | 2 |
| Wazuh Manager | MON-S2-01 | Site 2 | SIEM / Endpoint Security | 3 |
| Suricata | pfSense | Both | Network IDS/IPS | 3 |
| Grafana | MON-S2-01 | Site 2 | Dashboards | 3 |
| Prometheus | MON-S2-01 | Site 2 | Metrics Collection | 3 |
| Backup Repository | BACKUP-S2-01 | Site 2 | Off-site backups | 3 |
| Web Server | WEB-S2-01 | Site 2 | Internal web applications | 3 |
