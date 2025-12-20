# Single-Area OSPF with Inter-VLAN Routing on a Multilayer Switch

## Overview

This topology demonstrates inter-VLAN routing using a multilayer switch (MLS) with Single Area OSPF (Area 0) for upstream connectivity. All internal VLAN traffic is routed locally on the MLS using Switch Virtual Interfaces (SVIs). No router subinterfaces (Router-on-a-Stick) are used.

All routers in this topology are only being used for external or upstream network access and participation in OSPF, they do not handle inter-VLAN routing.

This design represents a scalable enterprise-style Layer 3 switching architecture suitable for high intra-VLAN traffic environments.

---

## Topology Description

### Physical Connections (Layer 1)

- MLS ↔ Router connected via Copper Cross-Over cable   
- MLS ↔ Access switches connected via Copper UTP cables  
- Hosts (PCs) connected to access switches using Copper UTP cables  

---

## Logical Design

### Layer 2

- Access switches operate purely at Layer 2
- Access switches connect to the MLS using 802.1Q trunk links
- Trunk between MLS0 and MLS1 carry VLANs 10, 20. 
- End devices connect to access ports mapped to their respective VLANs

### Layer 3

- The multilayer switch performs all inter-VLAN routing
- Each VLAN has a dedicated SVI acting as the default gateway
- No router subinterfaces are used
- OSPF Area 0 is used for dynamic routing to upstream networks

---

## Routing Protocol

- OSPF is configured in Single Area (Area 0)
- SVIs and routed uplink interfaces participate in OSPF
- Access VLAN interfaces are configured as passive interfaces
- The router exchanges routes dynamically with the MLS
- All routers and ML Switches (Layer 3) are configured to have last resort gateway set to ISPR1's interface

---

## Verification

The following checks confirm correct operation:

- Successful ping between hosts in VLAN 10, VLAN 20.
- Trunk links verified to carry VLANs 10, 20.
- SVIs are in Up/Up state with correct IP addressing
- Inter-VLAN traffic is routed locally by the MLS
- OSPF adjacencies are established successfully

---

## Design Notes

- Layer 3 switching removes the inter-VLAN bottleneck present in Router-on-a-Stick designs
- Layer 3 switching also allows vlan 10 to communicate with its hosts on different places, without requiring extra cables
- This architecture scales well in environments with high east-west traffic
- Centralized routing on the MLS reduces latency and improves performance
- The design follows enterprise best practices for campus networks

---

## Conclusion

This topology demonstrates a clean and scalable approach to inter-VLAN routing using a multilayer switch and Single Area OSPF. By routing traffic locally at Layer 3, the network avoids unnecessary router hops while maintaining dynamic routing for external connectivity.
