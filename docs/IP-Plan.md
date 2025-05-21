# IP Plan

All IPs are based on **10.x.x.x**, clean /30s for P2P, /24s for VLANs, and specific IPs for loopbacks and tunnels.

## Global Structure

| Zone            | Subnet         | Purpose                    |
| --------------- | -------------- | -------------------------- |
| Loopbacks       | 10.255.X.X/32  | Router-IDs, BGP            |
| P2P Links       | 10.100.X.X/30  | Point-to-point links       |
| Tunnel Intfs    | 10.200.X.X/24  | DMVPN tunnels (R1 + BRs)   |
| VLANs           | 10.10X.X.X/24  | SVI gateways               |
| DMZ Servers     | 10.150.0.0/24  | Syslog, DNS, SNMP, DHCP    |
| Mgmt & Mon      | 10.250.0.0/24  | SNMP, NetFlow, etc         |
| NAT Outside     | DHCP from net  | BR routers & Edge use this |

## Loopback IPs (/32)

| Device   | Loopback0 IP | Use                     |
| -------- | ------------ | ----------------------- |
| R1       | 10.255.0.1   | BGP Router-ID, OSPF RID |
| R2       | 10.255.0.2   | BGP Router-ID, OSPF RID |
| R-EDGE   | 10.255.0.3   | BGP Router-ID           |
| ISP1     | 10.255.0.11  | Test reachability       |
| ISP2     | 10.255.0.12  | Test reachability       |
| BR1-RTR  | 10.255.1.1   | EIGRP RID, DMVPN        |
| BR2-RTR  | 10.255.1.2   | EIGRP RID               |
| BR3-RTR  | 10.255.1.3   | EIGRP RID               |

## P2P Links (/30)

| Link               | Subnet         | IP Assignments           |
| ------------------ | -------------- | ------------------------ |
| R1 ↔ R2            | 10.100.0.0/30  | R1: .1, R2: .2           |
| R1 ↔ CSW1 (Po1)    | 10.100.0.4/30  | R1: .1, CSW1: .2         |
| R2 ↔ CSW2 (Po2)    | 10.100.0.8/30  | R2: .1, CSW2: .2         |
| R-EDGE ↔ FW inside | 10.100.0.12/30 | R-EDGE: .1, FW: .2       |
| FW ↔ R1 core       | 10.100.0.16/30 | FW: .1, R1: .2           |
| FW ↔ R2 core       | 10.100.0.20/30 | FW: .1, R2: .2           |
| R-EDGE ↔ ISP1      | 10.100.0.24/30 | R-EDGE: .1, ISP1: .2     |
| R-EDGE ↔ ISP2      | 10.100.0.28/30 | R-EDGE: .1, ISP2: .2     |
| BR1-RTR ↔ Internet | DHCP           | NATed real IP            |
| BR2-RTR ↔ Internet | DHCP           | NATed real IP            |
| BR3-RTR ↔ Internet | DHCP           | NATed real IP            |

## DMVPN Tunnels (R1 = Hub)

| Device    | Tunnel IP   |
| --------- | ----------- |
| R1 (Hub)  | 10.200.0.1  |
| BR1-RTR   | 10.200.0.11 |
| BR2-RTR   | 10.200.0.12 |
| BR3-RTR   | 10.200.0.13 |

## VLAN IPs (SVI)

| VLAN Purpose       | Subnet        | Gateway IP  | Devices               |
| ------------------ | ------------- | ----------- | --------------------- |
| VLAN 10 – Users    | 10.101.0.0/24 | 10.101.0.1  | ASW1/ASW2 (HSRP)      |
| VLAN 20 – Sales    | 10.102.0.0/24 | 10.102.0.1  | ASW1/ASW2 (HSRP)      |
| VLAN 99 – Mgmt     | 10.250.0.0/24 | 10.250.0.1  | CSW1/CSW2 (HSRP)      |

## DMZ Zone

| Device                   | IP Address   |
| ------------------------ | ------------ |
| SRV1 (Radius, DNS, DHCP) | 10.150.0.10  |
| SRV2 (Syslog, SNMPv3)    | 10.150.0.11  |
| DMZ SVI (Firewall side)  | 10.150.0.1   |

## Virtual IPs (HSRP / VRRP)

| VLAN | Virtual IP    | Routers              |
| ---- | ------------- | -------------------- |
| 10   | 10.101.0.254  | R1 & R2 or CSW1/CSW2 |
| 20   | 10.102.0.254  | R1 & R2              |
| 99   | 10.250.0.254  | R1 & R2              |