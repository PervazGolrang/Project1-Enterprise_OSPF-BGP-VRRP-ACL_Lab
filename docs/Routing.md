## OSPF

- Area 0: R1, R2, CSW1, CSW2
- Area 10: BR1-RTR tunnel
- Stub: BR1
- Redistribution:
  - EIGRP <-> OSPF on R1
  - BGP <-> OSPF on R-EDGE

## BGP

- R1, R2: AS 65001 (iBGP, R1 = RR)
- R-EDGE: AS 65010
- ISP1: AS 65011
- ISP2: AS 65012

# Routing – OSPF Setup (Phase 1: Below Firewall)

## Goal

Establish stable OSPF Area 0 routing between R1, R2, CSW1, and CSW2 to form the core backbone. This forms the foundation for later redistribution and DMVPN integration.

## Design

| Device | Area   | Loopback ID     |
|--------|--------|------------------|
| R1     | Area 0 | 10.255.0.1       |
| R2     | Area 0 | 10.255.0.2       |
| CSW1   | Area 0 | 10.255.0.3       |
| CSW2   | Area 0 | 10.255.0.4       |

## Interfaces in OSPF

| Device | Interfaces in OSPF                  |
|--------|-------------------------------------|
| R1     | Loopback0, G1 (to CSW1), Gx (to R2) |
| R2     | Loopback0, G1 (to CSW2), Gx (to R1) |
| CSW1   | Loopback0, G1 (to R1), G7 (to CSW2) |
| CSW2   | Loopback0, G1 (to R2), G7 (to CSW1) |

## Example Config (R1)
```bash
router ospf 1
 router-id 10.255.0.1
 network 10.255.0.1 0.0.0.0 area 0
 network 10.100.0.0 0.0.0.255 area 0


 
