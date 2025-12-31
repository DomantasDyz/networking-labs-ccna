# Dual-Stack IPv4 / IPv6 Flat Network with Multilayer Switches
## Overview

This lab demonstrates a **simple dual-stack IPv4 and IPv6 network** operating within a **single Layer-2 broadcast domain**. The design intentionally avoids VLAN segmentation and default gateways to focus on **fundamental networking concepts**, including Layer-2 switching, Layer-3 adjacency, efficient IPv4 subnetting using **VLSM**, and IPv6 address configuration using **EUI-64 and SLAAC**.

Multilayer switches are used primarily as **distribution devices**, while routing is provided by routers connected upstream. All end devices operate within the same IPv4 subnet and IPv6 prefix, allowing direct host-to-host communication without inter-subnet routing.

This lab emphasizes **clarity, correctness, and protocol behavior** rather than enterprise complexity.

---

## Topology Description

### Physical Connections (Layer 1)

- Router ↔ Router connected via Serial point-to-point links
- Router ↔ Multilayer switch connected via Copper Straight-Through
- Multilayer switches ↔ Access switches connected via Copper Straight-Through
- End hosts (PCs) connected to access switches using Copper Straight-Through

---

## Logical Design

### Layer 2

- All switches operate within **VLAN 1**
- No additional VLANs are configured
- The entire topology exists in **one broadcast domain**
- Access switches forward frames based purely on MAC addressing
- Spanning Tree Protocol ensures a loop-free topology

---

### Layer 3

- All end devices share a **single IPv4 subnet** created by using VLSM
- All devices share a **single IPv6 prefix**
- No inter-VLAN routing is required
- No default gateways are configured
- Routers participate in Layer-3 connectivity but are not required for intra-subnet communication

Because all hosts reside in the same network, communication occurs directly using:
- **ARP** for IPv4
- **Neighbor Discovery** for IPv6

---

## IPv4 Addressing and VLSM Design

The IPv4 addressing scheme is based on the parent network:

- **Parent network:** `192.168.100.0/24`

### Subnet Requirements

- Required **14 usable IPv4 addresses** for lab devices
- Additional **loopback addresses** for router identification and testing

### VLSM Allocation

- **Primary LAN subnet:**  
  - `192.168.100.0/28`  
  - Provides 14 usable host addresses  
  - Used by hosts, switches, and router LAN interfaces

- **Router loopback interfaces:**  
  - `192.168.100.16/32` (Router1 loopback0)  
  - `192.168.100.17/32` (Router2 loopback0)

This VLSM approach demonstrates efficient IPv4 address utilization by allocating only the required number of addresses per purpose.

---

## IPv6 Addressing Design

- All devices use a single IPv6 prefix:
  - `2001:db8:3445:ABDE::/64`

### Routers

- Router LAN interfaces use **EUI-64–derived IPv6 addresses**
- Resulting interface identifiers:
  - Router1 ends with `::1`
  - Router2 ends with `::2`
- This demonstrates automatic IPv6 interface ID generation based on MAC addresses

### Multilayer Switches

- Multilayer switches use **IPv6 address autoconfiguration (SLAAC)** on `VLAN 1`
- IPv6 addresses are learned dynamically via Router Advertisements
- No static IPv6 addresses are manually assigned on switch SVIs

IPv6 Neighbor Discovery handles address resolution, and no default route is required for local communication within the subnet.

---

## Routing Role of Routers

- Routers are connected to the LAN but do not act as default gateways for hosts
- A point-to-point WAN link (`203.0.113.0/30`) connects the routers
- Loopback interfaces are used to demonstrate logical addressing and testing
- The WAN and loopback networks exist independently of host communication

---

## Verification

The following checks confirm correct operation:

- Successful IPv4 pings between all hosts
- Successful IPv6 pings between all hosts
- Reachability of router loopback addresses
- Correct EUI-64–derived IPv6 addresses on router interfaces
- IPv6 autoconfig addresses present on multilayer switch VLAN 1
- All switch ports in **Up/Up** state
- Proper ARP and IPv6 Neighbor Discovery operation
- WAN link between routers operational

---

## Design Notes

- VLSM is used to efficiently subnet a `/24` IPv4 network based on actual host requirements
- IPv6 EUI-64 and SLAAC are used to demonstrate automatic address configuration
- A flat network design simplifies troubleshooting and highlights protocol fundamentals
- The absence of default gateways reinforces understanding of when routing is required
- Dual-stack configuration demonstrates IPv4 and IPv6 coexistence
- Multilayer switches are used for structure, not forced complexity
- This lab prioritizes **correct behavior over unnecessary segmentation**

---

## Conclusion

This lab demonstrates a **minimalist dual-stack network** with efficient IPv4 addressing using VLSM and native IPv6 support through EUI-64 and SLAAC. By operating within a single broadcast domain and avoiding unnecessary routing architecture, this design clearly illustrates how hosts communicate at Layer 2 and Layer 3 when no subnet boundaries exist.
