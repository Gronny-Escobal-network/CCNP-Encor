# 🔹 Lab 4 – OSPF Configuration & Validation
## ⚙️ Configuration
### 🔀 **Z14** 
```bash
interface Loopback57
 ip address 10.57.57.57 255.255.255.255
!
interface Ethernet1/1
 ip address 10.10.50.129 255.255.255.252
!
router ospf 100
 router-id 14.14.14.14
 area 1 virtual-link 21.21.21.21        <-- The right command
 network 10.0.64.0 0.0.15.255 area 1
 network 10.10.50.128 0.0.0.3 area 3
 network 10.57.57.57 0.0.0.0 area 3
```
### 🔀 **W25** 
```bash
interface Loopback201
 ip address 192.168.201.129 255.255.255.255
!
interface Ethernet0/0
 ip address 10.10.50.130 255.255.255.252
!
router ospf 100
 router-id 29.29.29.29
 network 10.10.50.128 0.0.0.3 area 3
 network 10.10.50.136 0.0.0.3 area 3
 network 192.168.201.129 0.0.0.0 area 3
```
### 🔀 **Z21** 
```bash
router ospf 100
 router-id 21.21.21.21
 area 1 virtual-link 14.14.14.14        <-- The right command     
 network 11.0.0.0 0.0.15.255 area 0
 network 12.0.0.0 0.0.15.255 area 1
```

-----

## 🔍 Validation

✅ Before (on Z11)
```bash
Z11#sh ip route 192.168.201.129 255.255.255.255 longer-prefixes

Gateway of last resort is 10.0.64.4 to network 0.0.0.0

```
✅ After summarization (on Z11)
```bash
Z11#sh ip route 192.168.201.129 255.255.255.255 longer-prefixes

Gateway of last resort is 10.0.64.4 to network 0.0.0.0

      192.168.201.0/32 is subnetted, 2 subnets
O IA     192.168.201.129 [110/21] via 10.0.64.3, 00:00:02, Ethernet0/0
!
Z11#ping 192.168.201.129
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.201.129, timeout is 2 seconds:
.....
Success rate is 0 percent (0/5)
Z11#ping 192.168.201.129
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.201.129, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/2 ms

```

-----

## 🖧 Topology

![Lab 1 Topology](../Diagrams/virtual.png)

-----
