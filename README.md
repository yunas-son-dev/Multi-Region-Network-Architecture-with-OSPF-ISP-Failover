# Multi-Region Network Architecture with OSPF & ISP Failover

## Overview
Designed and implemented a simulated multi-site enterprise network including a core data center and two regional branches. 

The architecture focuses on routing efficiency, network segmentation, ISP redundancy, and controlled Internet access via NAT.

---

## Architecture Goals
- Optimised IPv4 addressing using VLSM
- VLAN segmentation with 802.1Q trunking
- Inter-VLAN routing (Router-on-a-Stick)
- Single-area OSPFv2 for internal routing
- Dual ISP connectivity with automatic failover (floating static route)
- NAT overload (PAT) for internal hosts
- Static NAT for core service exposure

---

## Key Technical Components

### Layer 2
- VLAN segmentation (User / Management / Services)
- Native VLAN isolation
- Trunk configuration with restricted allowed VLAN list

### Layer 3
- Inter-VLAN routing via subinterfaces
- OSPF route advertisement
- Default route propagation

### Edge Connectivity
- Primary default route
- Floating static backup route
- Automatic ISP failover

### NAT
- PAT (overload) for outbound traffic
- Static NAT for internal server

---

## Verification
Validated routing tables, OSPF adjacencies, NAT translations, and end-to-end connectivity using IOS CLI verification commands.
