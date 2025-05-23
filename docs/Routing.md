# Routing

This lab uses OSPF as the IGP and eBGP between R-EDGE and both ISPs. Static routes are used for upstream redundancy via IP SLA. Redistribution is controlled to avoid loops.

## IGP: OSPF

* **Process ID**: 1
* **Router IDs**: Set manually based on Loopback0 (10.255.0.X)
* **Network Type**: point-to-point on all interfaces
* **Passive Interfaces**: Default passive; only P2P and Loopback0 links are non-passive
* **Areas**: All interfaces in Area 0 (flat OSPF)
* **Redistribution**:
  * `redistribute bgp 65001 subnets route-map BGP-DEFAULT`
  * `redistribute static subnets`
  * Default route injected with `default-information originate always`

## BGP

* **AS**: 65001 (R-EDGE)
* **Peers**:
  * ISP1: 193.182.25.2 (AS 65011)
  * ISP2: 185.75.210.10 (AS 65012)

* **Redistribution**:
  * Only default route advertised from ISPs to R-EDGE
  * BGP route into OSPF filtered by prefix list and route-map

## IP SLA

* Tracks 193.182.25.2 (ISP1 next hop)
* Replaces default route with ISP2 on failure
* Delay down 10s, up 5s

```
ip sla 1
 icmp-echo 193.182.25.2 source-interface GigabitEthernet1
  frequency 5
ip sla schedule 1 life forever start-time now
track 1 ip sla 1 reachability
 delay down 10 up 5
ip route 0.0.0.0 0.0.0.0 193.182.25.2 track 1
ip route 0.0.0.0 0.0.0.0 185.75.210.10 5
```

## Notes

* All `/30` links set as point-to-point in OSPF
* No redistribution into BGP from OSPF (one-way redistribution only)
* No use of PBR
* VRRP is used at the access layer (see VLANs.md)
