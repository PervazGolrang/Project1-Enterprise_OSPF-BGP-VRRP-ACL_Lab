## VLANs

| VLAN | Name        | Subnet           | Gateway       | Purpose     |
|------|-------------|------------------|---------------|-------------|
| 10   | Users       | 10.101.0.0/24    | .254 (VRRP)   | PC1, PC3    |
| 20   | Sales       | 10.102.0.0/24    | .254 (VRRP)   | PC2, PC4    |
| 30   | Servers     | 10.150.0.0/24    | .254 (VRRP)   | Ubuntu SRV  |
| 99   | Mgmt        | 10.250.0.0/24    | .254 (VRRP)   | SNMP/Syslog |