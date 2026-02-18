# Multi-Region Network Architecture with OSPF & ISP Failover

## Project Overview

This project demonstrates the design and implementation of a multi-region enterprise network architecture featuring:

- Custom VLSM subnet planning
- OSPF dynamic routing
- VLAN segmentation
- Dual-ISP internet failover
- Redundant WAN topology

The network was built from an initial base addressing requirement (172.16.0.0/16), and all subnet allocations were planned using CIDR and VLSM to meet host requirements efficiently.

---

## Network Topology

The architecture consists of:

- **Core-RT**
- **Branch-A-RT**
- **Branch-B-RT**
- Dual ISP connections (Primary / Backup)
- VLAN-based internal segmentation
- Point-to-point WAN links between all routers

Topology design discussion and configuration notes:

Design & Implementation Notes:  
https://github.com/user-attachments/assets/53cede5a-5f19-4fc0-9646-414028943f82

This issue documents routing behavior observations, adjacency validation details, and failover testing results during implementation.

---

## IP Address Planning & VLSM Design

Base Network:
```
172.16.0.0/16
```

Subnet allocation was designed based on host requirements per segment.

### Subnet Allocation Summary

| Subnet | Network |
|--------|---------|
| 172.16.64.0/24 | Branch-A VLAN10 |
| 172.16.65.0/25 | Branch-B VLAN10 |
| 172.16.65.128/26 | Branch-A VLAN20 |
| 172.16.65.192/27 | Core-RT DC |
| 172.16.65.224/28 | Branch-B VLAN20 |
| 172.16.65.240/29 | Branch-A Management |
| 172.16.65.248/30 | Branch-B Management |
| 172.16.65.252/30 | P2P: Core ↔ Branch-A |
| 172.16.66.0/30 | P2P: Core ↔ Branch-B |
| 172.16.66.4/30 | P2P: Branch-A ↔ Branch-B |
| 203.0.113.0/30 | ISP Primary |
| 203.0.113.4/30 | ISP Secondary |

Design Decisions:
- Larger subnets assigned first
- WAN links use /30 for address efficiency
- Management networks isolated
- Proper binary boundary alignment maintained
- No overlapping subnets

This demonstrates structured VLSM-based CIDR planning.

---

## OSPF Dynamic Routing

- Process ID: 1
- Area: 0
- Explicit Router IDs:
  - Core-RT → 1.1.1.1
  - Branch-A-RT → 2.2.2.2
  - Branch-B-RT → 3.3.3.3
- FULL adjacency verified

Validation:
```
show ip ospf neighbor
show ip route
```

OSPF-learned routes correctly reflected across all routers.

---

## VLAN Segmentation & Router-on-a-Stick

Implemented:
- 802.1Q trunking
- Subinterfaces per VLAN
- Inter-VLAN routing
- Proper gateway assignment
- Switch management VLAN configuration

Connectivity between internal segments verified.

---

## Dual ISP Internet Failover

Configured on Core-RT:

Primary route:
```
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

Secondary route (Higher AD):
```
ip route 0.0.0.0 0.0.0.0 203.0.113.5 10
```

Failover Testing Procedure:
1. Verified connectivity via Primary ISP
2. Shut down primary interface
3. Confirmed automatic switchover
4. Validated successful ping & traceroute

Result:
Redundant internet connectivity maintained without manual reconfiguration.

---

## Validation & Troubleshooting

Performed:

- Routing table inspection
- OSPF adjacency validation
- Static route prioritization testing
- Traceroute path verification
- Controlled interface failure simulation

All tests confirmed expected routing behavior.

---

## Repository Structure

```
configs/
  ├── Core-RT.txt
  ├── Branch-A-RT.txt
  └── Branch-B-RT.txt

verification/
  ├── ospf-neighbors.txt
  ├── routing-tables.txt
  └── failover-test.txt
```
---

## Engineering Concepts Demonstrated

- VLSM subnet planning
- CIDR-based IP allocation
- OSPF configuration & adjacency validation
- Administrative Distance manipulation
- WAN redundancy design
- VLAN trunking & inter-VLAN routing
- Routing table analysis
- Structured validation methodology

---

## What This Project Demonstrates

This project reflects practical understanding of:

- Enterprise WAN architecture
- Scalable IP address planning
- Dynamic routing behavior
- Redundant internet edge design
- Network resilience engineering
- CLI-based troubleshooting workflows

---

## Author

Yuna Son

Network & Infrastructure Engineering Focus  
Master of Information Technology UTS