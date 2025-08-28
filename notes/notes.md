## Final Notes

### Topology Adjustments

* Removed BR1, BR2, BR3, and Internet (CVR1000) router (DMVPN)
* Removed all DMVPN/IKEv2 tunnels and EIGRP processes

### R-EDGE

* Uses static routes with IP SLA to switch between ISP1 and ISP2
* Redistributes BGP (default only) and static into OSPF
* ACLs applied on Gi1 (ISP1), Gi2 (ISP2), and Gi4 (DMZ)
* Route-map `BGP-DEFAULT` filters to only send 0.0.0.0/0

## VRRP

* Active on all VLANs (10, 20)
* CSW1 SVI = .253, CSW2 SVI = .252, VRRP VIP = .254 (CSW2 priority 110)

## Core Switches (CSW1, CSW2)

* Do NOT support switchport
* Use ROAS for VLAN subinterfaces
* ECMP setup using Gi4 interfaces only (no EtherChannel/LAG)

### Access Switches (ASW1–4)

* Trunked to both CSW1 and CSW2
* VLANs statically configured
* Port-Security, BPDU Guard, and PortFast enabled on all PC ports

### ACLs

* `ISP_INBOUND`: Permits BGP and ICMP, blocks all else
* ACLs applied **inbound** on correct interfaces (ISP)

### What’s Working

* OSPF and BGP routes
* Failover between ISPs
* VLAN trunking and L3 inter-VLAN routing
* VRRP and ACLs working correctly

### What’s Not Included

* DMVPN Phase 3 (abandoned due to config/scale issues)
* Linux services (too heavy for lab)
* NAT or any real Internet traffic

### Suggestions for Expansion

* Add NAT with overload at R-EDGE for outbound
* Add SNMP monitoring and syslog server
* Add Netflow monitoring
* Add DMZ firewall (e.g. ASA, Fortinet)
* Add Phase-3 DMVPN with IKEv2
* Add CoPP on R-EDGE 
* Add uRPF for R-EDGE
* Add wireshark captures
* Add .yaml files
