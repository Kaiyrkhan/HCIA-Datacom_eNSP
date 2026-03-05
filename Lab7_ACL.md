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

## Step3: Configure Remote Access (Telnet)

R2
```shell
telnet server enable
user-interface vty 0 4
 user privilege level 3
 set authentication password cipher Huawei@123
```

## Step4: Create ACL for Telnet

R2
```shell
acl 3001
 rule 5 permit tcp source 50.2.2.2 0 destination 10.1.1.102 0 destination-port eq 23
 rule 10 deny tcp source any
 display this

user-interface vty 0 4
acl 3001 inbound
немесе
int g0/0/1
traffic-filter inbound acl 3001

display acl 3001
display cu section acl
```

Verification:
```shell
<R1> telnet -a 10.1.1.101 10.1.1.102
<R1> telnet -a 50.1.1.1 10.1.1.102
<R1> telnet -a 50.2.2.2 10.1.1.102
```

## Step5: Create ACL for ICMP

R1
```shell
<R1> ping -a 10.1.1.101 10.1.1.102
<R1> ping -a 50.1.1.1 10.1.1.102
<R1> ping -a 50.2.2.2 10.1.1.102
```

R2
```shell
acl 3002
 rule 5 deny icmp source 50.2.2.2 0 destination 10.1.1.102 0
 rule 10 permit icmp source any

int g0/0/1
traffic-filter inbound acl 3002

display acl 3002
display cu section acl
```

Verification:
```shell
<R1> ping -a 10.1.1.101 10.1.1.102
Reply from 10.1.1.102: bytes=56 Sequence=1 ttl=255 time=20 ms

<R1> ping -a 50.1.1.1 10.1.1.102
Reply from 10.1.1.102: bytes=56 Sequence=1 ttl=255 time=20 ms

<R1> ping -a 50.2.2.2 10.1.1.102
Request time out
```

