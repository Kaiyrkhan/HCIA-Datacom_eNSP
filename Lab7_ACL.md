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

## Step1: Configure IP address

R1
```shell
undo terminal monitor
system-view
sysname R1
int g0/0/0
 ip address 10.1.1.101 30
int loopback0
 ip address 50.1.1.1 32
int loopback1
 ip address 50.2.2.2 32
quit
```
R2
```shell
undo terminal monitor
system-view
sysname R3
int g0/0/1
 ip address 10.1.1.102 30
quit
```

## Step2: Configure OSPF

R1
```shell
ospf
area 0
network 50.1.1.1 0.0.0.0
network 50.2.2.2 0.0.0.0
network 10.1.1.100 0.0.0.3
return
```
R2
```shell
ospf
area 0
network 10.1.1.100 0.0.0.3
return
```
```shell
ping 50.1.1.1
ping 50.2.2.2
ping 10.1.1.101
```

## Result:
**Telnet**  
telnet -a 10.1.1.101 10.1.1.102 (deny)  
telnet -a 50.1.1.1 10.1.1.102 (deny)  
telnet -a 50.2.2.2 10.1.1.102 (permit)  
**ICMP**  
ping -a 50.2.2.2 10.1.1.102 (deny)  
ping -a 50.1.1.1 10.1.1.102 (permit)  
ping -a 10.1.1.101 10.1.1.102 (permit)  


```shell
```
