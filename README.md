# Full Enterprise Lab - Project 1

This is a complete simplistic CCNP enterprise network simulation using Cisco devices in EVE-NG.

## Topology Overview
Includes:
- OSPF (Area 0)
- eBGP between R-EDGE and ISPs
- IP SLA + Tracking for dual ISP failover
- VRRP between CSW1/CSW2
- ACLs for ISP and DMZ control
- Port security, STP (portfast, bpduguard)

## Topology
[`full-topology-drawio.drawio`](topology/full-topology-drawio.drawio)

## Directory Layout
- `configs/`: Device configurations
- `docs/`: Documentation
- `topology/`: Draw.io diagram
- `notes/`: Scratch notes

## Configurations

The `configs/` directory contains raw text-based configurations for each network device used in the lab. These were manually written and verified during lab creation.

- Compatible with EVE-NG
- Contains interface configs, routing protocols, ACLs, and VRRP
- Devices: R1, R2, R-EDGE, ISP1, ISP2, ASW1–4, CSW1, CSW2

## Network Devices:
- CSR1000v
- vIOS Switch
- vIOS Router
- Virtual PC (VPC)
