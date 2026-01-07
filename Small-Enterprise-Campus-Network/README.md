# Small Enterprise Campus Network lab with Redundancy and Centralized Services

## Overview

This lab implements a realistic enterprise campus network using a hierarchical design model.  
The topology demonstrates VLAN segmentation, multilayer switching, dynamic routing (OSPF), gateway redundancy (FHRP), centralized DHCP/DNS services and access control.

The network is designed to provide high availability, controlled inter-VLAN communication, and centralized infrastructure services while maintaining clear separation between departments.

---

## Topology Description

### Physical Connections (Layer 1)

- Router ↔ Router connected via point-to-point routed links
- Router ↔ Multilayer switches connected via routed GigabitEthernet interfaces
- Multilayer switches ↔ Access switches connected via 802.1Q trunk links
- End hosts (PCs) connected to access switches via access ports
- Dedicated server connected through an access switch to a multilayer switch

---

## Logical Design

### Layer 2

- VLAN-based segmentation per department
- Access switches operate using access ports for end devices
- Trunk links carry multiple VLANs between access and distribution layers
- Spanning Tree Protocol (STP) ensures loop-free topology and provides backup links, if main ones fail.
- VLAN traffic separation enforced at Layer 2

### Layer 3

- Inter-VLAN routing performed on multilayer switches using SVIs
- First Hop Redundancy Protocol (FHRP) provides gateway redundancy, which means only one virtual gateway is being used for both MLSs
- OSPF used as the interior gateway routing protocol
- Routed point-to-point links use /30 subnets to minimize broadcast domains
- Centralized routing ensures full network reachability

---

## VLAN and IPv4 Addressing Design

The IPv4 addressing scheme uses efficient subnet allocation per VLAN.

### VLAN Allocation

| VLAN | Purpose        | Subnet            |
|-----:|---------------|-------------------|
| 10   | Marketing     | 192.168.10.0/27   |
| 20   | Users         | 192.168.20.0/27   |
| 30   | Server VLAN   | 10.10.30.0/30     |
| 67   | IT Department | 172.30.67.0/29    |

- VLAN gateways are implemented as SVIs on multilayer switches
- FHRP virtual IPs are used as default gateways for hosts
- /30 subnets are used for routed inter-device links and for the server, because there only two devices (MLS gateway and the server itself) that requires an IP address

---

## Routing Design (OSPF) - Area 0

- OSPF is deployed across routers and multilayer switches
- All VLAN and routed networks are advertised dynamically
- Provides fast convergence and scalable routing
- Eliminates the need for static routes
- Ensures full connectivity between all network segments

---

## DHCP and DNS Services

A centralized server in VLAN 30 provides infrastructure services.

### DHCP

- Separate DHCP pools created for each VLAN
- Correct default gateway, subnet mask, and DNS server assigned
- `ip helper-address` configured on all user VLAN SVIs
- Hosts dynamically receive IP configuration

### DNS

- Internal DNS service enabled on the server
- Static A records created for internal hosts
- DNS server address distributed via DHCP
- Hosts resolve names across VLANs

---

## Access Control Lists (ACLs)

Extended ACLs are used to enforce security policies:

- Restrict specific VLANs (in this lab - Users) from accessing DNS services
- ACLs applied inbound on Users VLAN SVIs
- Ensures segmentation and controlled service access

---

## Verification

The following checks confirm correct operation:

- Successful DHCP address assignment for all hosts
- Inter-VLAN communication through multilayer switches
- Gateway redundancy operation via FHRP
- Dynamic routing confirmed using OSPF
- DNS name resolution across VLANs
- ACLs correctly permitting and denying traffic
- End-to-end connectivity across the entire topology

---

## Design Notes

- VLANs provide logical segmentation by department
- Multilayer switches combine Layer 2 switching and Layer 3 routing
- FHRP ensures high availability for default gateways
- OSPF provides scalable and resilient routing
- Centralized DHCP and DNS simplify host management
- ACLs enforce security at the network edge
- Routed /30 links reduce unnecessary broadcast traffic
- The design balances realism with clarity for learning purposes

---

## Conclusion

This lab demonstrates a complete small enterprise-style campus network with redundancy, dynamic routing, centralized services, and security controls.  

