# IP Plan

All IPs are based on **10.x.x.x**, clean /30s for P2P, /24s for VLANs, and specific IPs for loopbacks and tunnels.

## Global Structure

| Zone            | Subnet         | Purpose                    |
| --------------- | -------------- | -------------------------- |
| Loopbacks       | 10.255.X.X/32  | Router-IDs, BGP            |
| P2P Links       | 10.100.X.X/30  | Point-to-point links       |
| VLANs           | 10.10X.0.X/24  | SVI gateways, ex/ VLAN 50  |
| Tunnel Intfs    | 10.200.0.X/24  | DMVPN tunnels (R1 + BRs)   |
| DMZ Servers     | 10.150.0.0/24  | Syslog, DNS, SNMP, etc     |

## Loopback IPs (/32)

| Device   | Loopback0 IP | Use                     |
| -------- | ------------ | ----------------------- |
| R1       | 10.255.0.1   | BGP Router-ID, OSPF RID |
| R2       | 10.255.0.2   | BGP Router-ID, OSPF RID |
| CSW1     | 10.255.0.3   | OSPF RID                |
| CSW2     | 10.255.0.4   | OSPF RID                |
| ASW1     | 10.255.0.5   | Loopback0               |
| ASW2     | 10.255.0.6   | Loopback0               |
| ASW3     | 10.255.0.7   | Loopback0               |
| ASW4     | 10.255.0.8   | Loopback0               |
| R-EDGE   | 10.255.0.9   | BGP Router-ID, HUB      |
| ISP1     | 10.255.0.11  | Test reachability       |
| ISP2     | 10.255.0.12  | Test reachability       |
| DMZ-SW   | 10.255.0.13  | Test reachability       |
| BR1-RTR  | 10.255.1.1   | EIGRP RID, DMVPN        |
| BR2-RTR  | 10.255.1.2   | EIGRP RID, DMVPN        |
| BR3-RTR  | 10.255.1.3   | EIGRP RID, DMVPN        |

## P2P Links (/30)

| Link               | Subnet          | IP Assignments           |
| ------------------ | --------------- | ------------------------ |
| R1 ↔ R2            | 10.100.0.0/30   | R1: .1, R2: .2           |
| R1 ↔ CSW1          | 10.100.0.4/30   | R1: .5, CSW1: .6         |
| R1 ↔ CSW2          | 10.100.0.8/30   | R1: .9, CSW2: .10        |
| R2 ↔ CSW1          | 10.100.0.12/30  | R2: .13, CSW1: .14       |
| R2 ↔ CSW2          | 10.100.0.16/30  | R2: .17, CSW2: .18       |
| R-EDGE ↔ R1 Core   | 10.100.0.20/30  | R-EDGE: .21, R1: .22     |
| R-EDGE ↔ R2 Core   | 10.100.0.24/30  | R-EDGE: .25, R2: .26     |
| R-EDGE ↔ DMZ-SW    | 10.150.0.0/30   | R-EDGE: .1, DMZ-SW: .X   |
| ISP1 ↔ Internet    | 10.100.1.0/30   | ISP1: .1, Internet: .2   |
| ISP2 ↔ Internet    | 10.100.2.0/30   | ISP2: .1, Internet: .2   |
| R-EDGE ↔ ISP1      | 193.182.25.0/30 | R-EDGE: .1, ISP1: .2     |
| R-EDGE ↔ ISP2      | 185.75.210.8/30 | R-EDGE: .9, ISP1: .10    |
| BR1-RTR ↔ Internet | DHCP            | NATed real IP            |
| BR2-RTR ↔ Internet | DHCP            | NATed real IP            |
| BR3-RTR ↔ Internet | DHCP            | NATed real IP            |

## DMVPN Tunnels (R1 = Hub)

| Device    | Tunnel IP   |
| --------- | ----------- |
| R1 (Hub)  | 10.200.0.1  |
| BR1-RTR   | 10.200.0.11 |
| BR2-RTR   | 10.200.0.12 |
| BR3-RTR   | 10.200.0.13 |GIVE M

## VLAN IPs (VRRP on CSW1/CSW2)

| VLAN Purpose       | Subnet        | Gateway IP    | Devices               |
| ------------------ | ------------- | ------------- | --------------------- |
| VLAN 10 – Users    | 10.101.0.0/24 | 10.101.0.254  | ASW1/ASW2 (PC1, PC3)  |
| VLAN 20 - Sales    | 10.102.0.0/24 | 10.102.0.254  | ASW1/ASW2 (PC2, PC3)  |
| VLAN 50 – Servers  | 10.150.0.0/24 | 10.102.0.254  | ASW1/ASW2 (SRV1, SRV2)|

## DMZ Zone

| Device                   | IP Address   |
| ------------------------ | ------------ |
| SRV1 (Radius, DNS, DHCP) | 10.150.0.10  |
| SRV2 (Syslog, SNMPv3)    | 10.150.0.11  |
| DMZ SVI (Firewall side)  | 10.150.0.1   |

## Virtual IPs (VRRP)

| VLAN | Virtual IP    | Routers      |
| ---- | ------------- | -------------| 
| 10   | 10.101.0.254  | CSW1 & CSW2  |
| 20   | 10.102.0.254  | CSW1 & CSW2  |
| 50   | 10.150.0.254  | CSW1 & CSW2  |