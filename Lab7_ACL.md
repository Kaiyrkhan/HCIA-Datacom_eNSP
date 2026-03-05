# Access Control List (ACL) Configuration on Huawei Router

### 🖧 Network Topology
![Topology](images/Lab7_NetworkTopology_ACL.png)  
[Download Link for eNSP Topology File](Topology/Lab7_NetworkTopology_ACL.topo)

## Scenario:
1) Configure IP address;
2) Configure OSPF;
3) Configure Remote Access (Telnet);
4) Create ACL;
5) Verification.

## Result:

**Telnet**  
telnet -a 10.1.1.101 10.1.1.102 (deny)  
telnet -a 50.1.1.1 10.1.1.102 (deny)  
telnet -a 50.2.2.2 10.1.1.102 (permit)  

**ICMP**  
ping -a 50.2.2.2 10.1.1.102 (deny)  
ping -a 50.1.1.1 10.1.1.102 (permit)  
ping -a 10.1.1.101 10.1.1.102 (permit)  
