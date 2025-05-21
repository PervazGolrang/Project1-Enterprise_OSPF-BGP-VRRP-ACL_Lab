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
