# GNS3 Lab: OSPF, SNMP and iperf3 Bandwidth Monitor

Two Cisco routers (R1, R2) run OSPF area 0 and SNMP. Two Linux IPTerm nodes
generate a 5 Mbps iperf3 stream. A Python script polls the router's
ifInOctets/ifOutOctets counters over SNMP and prints live Mbps.

## Topology
Cloud1 - Switch1 - R1 (f0/0) and R2 (f0/0)   (DHCP uplinks)
R1 f1/0 - R2 f1/0   10.12.0.0/24             (OSPF link)
R1 f1/1 - ipterm-1  172.16.0.0/24
R2 f1/1 - ipterm-2  172.16.1.0/24

## Files
- r1.cfg, r2.cfg: router configurations
- ipterm-setup.txt: host addressing and iperf3 commands
- get_traffic.py: SNMP polling script

## Run
1. Apply r1.cfg and r2.cfg and verify: show ip ospf neighbor
2. Configure the IPTerm nodes (ipterm-setup.txt)
3. Set IP in get_traffic.py to R1's f0/0 DHCP address
4. Find the interface index: snmpwalk -v2c -c public -On <R1 IP> 1.3.6.1.2.1.2.2.1.2
5. Start iperf3 server, then python3 get_traffic.py, then the iperf3 client

## Expected result
About 5.4 Mbps outbound and under 0.1 Mbps inbound on R1 Fa1/0.
