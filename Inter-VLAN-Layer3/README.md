# Inter-VLAN Routing – Layer 3 Switching

## Overview
Inter-VLAN routing is handled locally on the multilayer switch (MLS) using SVI interfaces as default gateways for VLAN 10, VLAN 20, and VLAN 30. No router subinterfaces are used for internal traffic. The router is only present for external network access if needed.

## Topology

# Layer 1 connections:
- MLS -> Router is connected with Copper UTP cable;
- MLS -> Hosts (PCs) are connected with Copper UTP cables;
- Both Switches are connected with Copper Cross-Over cable, due to the lack of Auto-MDIX technology.  

# Topology Summary
- Layer 2 access switches trunk to the MLS
- MLS performs routing between VLAN subnets using SVIs
- VLAN 10, VLAN 20, and VLAN 30 mapped to dedicated access ports

## VLAN Summary
- VLAN 10: 10.0.0.0/26 (Gateway 10.0.0.62) - Using last possible IP address as a gateway address for that VLAN.
- VLAN 20: 10.0.0.64/26 (Gateway 10.0.0.126) - Using last possible IP address as a gateway address for that VLAN.
- VLAN 30: 10.0.0.128/26 (Gateway 10.0.0.190) - Using last possible IP address as a gateway address for that VLAN.


## Verification
- Successful ping between VLAN 10, VLAN 20, VLAN 30
- Trunk links carrying VLANs 10/20/30 confirmed
- SVIs show Up/Up with correct addressing
- Traffic routed internally by the MLS


## Notes
This design is more scalable than Router-on-a-Stick (ROAS) in environments where VLAN traffic volume is high. Layer 3 switching removes inter-VLAN bottlenecks and allows routing to occur directly in hardware rather than passing inter-VLAN traffic through a single router subinterface.

