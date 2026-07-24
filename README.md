# CyberCorp Lab — Project Documentation

**Owners:** Josh (Networking/Infrastructure) & Drake (Cybersecurity)
**Started:** 07/24/2026
**Status:** Planning / Phase 1

---

## 1. Project Summary

A two-site distributed lab environment simulating a small enterprise ("CyberCorp") split across two physical locations. Josh's apartment operates as Site 1 (primary infrastructure), Drake's apartment operates as Site 2. Josh builds and operates infrastructure; Drake secures and tests it. The environment grows in phases, with recurring team exercises (Attack vs Defend, Disaster Recovery, Ticket System) layered in as services come online.

**Why this project:** Builds real, demonstrable experience in network design, Linux administration, routing, VPNs, and monitoring (Josh) and vulnerability management, detection engineering, and incident response (Drake), while practicing how networking and security teams actually collaborate.

---

## 2. Site Roles

| Site | Owner | Primary Responsibility |
|---|---|---|
| Site 1 — Josh's Home | Josh | Core infrastructure: AD, Linux servers, Docker, file server |
| Site 2 — Drake's Home | Drake | Security/monitoring stack: Wazuh, Suricata, Grafana, backups, web server |

---

## 3. Architecture Decisions

### 3.1 IP Addressing & Summarization
**Decision:** Each site gets its own /16 block so it can be advertised as a single summarized route across the VPN.
- Site 1 (Josh): `10.10.0.0/16`
- Site 2 (Drake): `10.20.0.0/16`
- VPN transit network: `10.99.0.0/30` (point-to-point tunnel)

**Rationale:** Avoids advertising a dozen individual /24s over the tunnel; keeps routing tables clean; leaves room to add VLANs later without re-addressing.

### 3.2 Segmentation Model
**Decision:** Four VLANs per site, based on trust level rather than just device type.

| VLAN | Purpose | Site 1 Subnet | Site 2 Subnet | Trust Level |
|---|---|---|---|---|
| 10 | Users | 10.10.10.0/24 | 10.20.10.0/24 | Medium |
| 20 | Servers | 10.10.20.0/24 | 10.20.20.0/24 | Medium-High |
| 30 | Management | 10.10.30.0/24 | 10.20.30.0/24 | Highest — most restricted |
| 40 | Guest | 10.10.40.0/24 | 10.20.40.0/24 | Lowest — isolated |

**Rationale:** Guest never reaches Servers or Mgmt. Mgmt is the most locked down, unreachable except from a designated admin host.

### 3.3 Routing Protocol
**Decision:** Run OSPF between sites over the VPN tunnel rather than static routes.

**Rationale:** Static routing would be simpler and is what you'd likely do in production for just 2 sites, but I decided to run OSPF here to learn about dynamic routing.

### 3.4 VPN Topology
**Decision:** Single WireGuard site-to-site tunnel

**Rationale:** WireGuard chosen over IPSec for simpler configuration and troubleshooting while learning. Revisit if a 3rd site is added — would need to decide hub-and-spoke vs full mesh at that point.

### 3.5 Routing/Firewall Colocation
**Decision:** pfSense handles both firewall and routing/inter-VLAN routing at each site (no separate router/L3 switch).

**Rationale:** Standard for small-scale deployments; separating the functions would be over-engineering at this size.

### 3.6 Management Plane Isolation
**Decision:** pfSense admin interfaces and switch management live only on the Mgmt VLAN, not reachable from Users/Guest directly.

**Rationale:** Commonly overlooked in practice, commonly tested by real attackers.

---

## 4. Open Decisions / To Revisit
- [ ] HA/failover for firewalls — deferred to Phase 4
- [ ] Internal PKI — deferred until AD is stable
- [ ] 3rd site — revisit VPN topology decision if this happens

---

## 5. Document Index
- `README.md` — this file
- `00-network-diagram.pdf` — visual network topology
- `01-ip-addressing-plan.md` — full subnet/VLAN reference table
