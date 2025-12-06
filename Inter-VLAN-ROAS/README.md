# Inter-VLAN Routing – Router-on-a-Stick (ROAS)

## Overview
Inter-VLAN routing performed using a single router interface (GigabitEthernet0/0) with 802.1Q subinterfaces for VLAN 10, VLAN 20, and VLAN 30. Each VLAN uses the router subinterface as its default gateway.

## Topology

- Switch to router link configured as trunk
- Router handles routing between VLAN subnets
- Access ports assigned to VLANs 10, 20, and 30

## VLAN Summary
VLAN 10: 10.0.0.0/26 (Gateway 10.0.0.62) - Using last possible IP address as a gateway address for that VLAN.
VLAN 20: 10.0.0.64/26 (Gateway 10.0.0.126) - Using last possible IP address as a gateway address for that VLAN.
VLAN 30: 10.0.0.128/26 (Gateway 10.0.0.190) - Using last possible IP address as a gateway address for that VLAN.

## Key Configuration

# Router
interface g0/0.10
 encapsulation dot1Q 10
 ip address 10.0.0.62 255.255.255.192

interface g0/0.20
 encapsulation dot1Q 20
 ip address 10.0.0.126 255.255.255.192

interface g0/0.30
 encapsulation dot1Q 30
 ip address 10.0.0.190 255.255.255.192

# Switch1 trunk
interface g0/1
 switchport mode trunk
 switchport trunk allowed vlan 10,30

interface g0/2
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30

## Verification
- Successful ping between VLAN 10, VLAN 20, VLAN 30
- Trunk link from Switch1 to Router passing VLANs 10/20/30
- Router subinterfaces reachable


## Notes
ROAS is suitable for smaller VLAN deployments. For larger environments or higher traffic needs, multilayer switching (SVI routing) is typically more efficient (ROAS in such cases can cause network congestions).
