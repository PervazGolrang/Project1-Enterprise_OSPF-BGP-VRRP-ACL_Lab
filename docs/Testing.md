## Test Areas and Results

### 1. Routing Convergence

* **OSPF full adjacency** verified on R-EDGE ↔ Core (R1/R2)
* **Static + IP SLA** tested by shutting down ISP1 interface; failover to ISP2 worked
* **BGP established** with both ISPs
* **Default route** redistributed into OSPF and verified as O E2 in core

### 2. VRRP

* VRRP on all VLANs tested by failing CSW1
* CSW2 correctly took over as `.254` IP owner
* Verified by `ping` from PCs and `show vrrp`

### 3. ACL Filtering

* Blocked unauthorized traffic:

  * ISP -> Inside (except BGP and ICMP)
  * Internet to DMZ allowed only specific ports
* Verified using `debug` and logs

### 4. Port Security and STP

* Verified Port-Security triggers with `violation restrict`
* Enabled `portfast` + `bpduguard` on all PC-facing ports
* Confirmed interface err-disabled on BPDU injection

### 5. End-to-End Reachability

* PC1–PC4 tested with manual IPs
* Can reach their VLAN gateway (VRRP IP)
* No Internet access (by design)
* Trunking and ROAS working properly

### 6. Stability Tests

* Let lab run for >20min after final setup
* No flapping interfaces or neighbor resets

### Removed

* DMVPN Phase 3: removed due to debugging issues

### Final Summary

All core, edge, and access features function as expected.
This lab simulates realistic enterprise failover, routing, and filtering behavior using only basic tools.
