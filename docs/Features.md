## High-Level Network Features

### 1. Core Redundancy

* Dual core switches (CSW1 & CSW2)
* ECMP (Equal-Cost Multi-Path) using `Gi4` link
* VRRP between CSW1 and CSW2

### 2. Access Layer Redundancy

* All ASWs connected to both CSW1 and CSW2
* Dual uplinks per ASW
* Trunks used for VLANs 10, and 20

### 3. Layer 3 Routing

* OSPF as the IGP
* BGP used between R-EDGE and ISP1/ISP2
* Static route with IP SLA for WAN failover
* Route redistribution with route-maps

### 4. Security Features

* Inbound ACLs on R-EDGE:
  * ISP_INBOUND: Allows only BGP, ICMP, and specific loopback access
* Port-security on ASW ports
* BPDU Guard + PortFast Edge on all end-user access ports

### 5. High Availability

* VRRP configured across all VLANs with pre-emption
* IP SLA + track on R-EDGE with floating static route

### 6. Management and Planning

* All IPs documented in [`IP-Plan.md`](/docs/IP-Plan.md)
* Static subnets for point-to-point links, loopbacks, and VLANs

### 7. Lab Goals Completed

* Full redundant core/edge/ISP simulation
* ACL filtering and WAN failover simulated
* No use of NAT, DNS, DHCP, AAA, or Internet access

### 8. Technology Stack Summary

| Protocol | Purpose                     |
| -------- | --------------------------- |
| OSPF     | IGP between R-EDGE and core |
| BGP      | External routing with ISPs  |
| VRRP     | Gateway redundancy per VLAN |
| IP SLA   | WAN failover detection      |
| ACLs     | WAN protection              |
