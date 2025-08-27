# 🔹 Lab 2 – BGP Configuration & Validation
## ⚙️ Configuration
### 🔀 **R60** 
```bash
interface Loopback60
 ip address 60.60.60.60 255.255.255.255
!
interface Loopback660
 no ip address
 ipv6 address 60::60/128
!
interface Ethernet0/0
 ip address 192.168.113.60 255.255.255.0
!
interface Ethernet0/1
 ip address 192.168.112.60 255.255.255.0
!
interface Ethernet0/2
 ip address 192.168.110.60 255.255.255.0
 ipv6 address FE80::60:64:60 link-local
 ipv6 address 2001:60:64::60/64
!
interface Ethernet0/3
 ip address 192.168.117.60 255.255.255.0
 ipv6 address FE80::61:60:60 link-local
 ipv6 address 2001:61:60::60/64
!
router ospf 100
 router-id 60.60.60.60
 network 192.168.110.0 0.0.0.255 area 0
!
router bgp 65201
 bgp router-id 60.60.60.60
 bgp log-neighbor-changes
 network 60.60.60.60 mask 255.255.255.255
 neighbor 192.168.110.64 remote-as 65201
 neighbor 192.168.110.64 next-hop-self
 neighbor 192.168.117.61 remote-as 65200
```
### 🔀 **R63** 

```bash
interface Loopback63
 ip address 63.63.63.63 255.255.255.255
!
interface Loopback663
 no ip address
 ipv6 address 63::63/128
!
interface Ethernet0/0
 ip address 192.168.115.63 255.255.255.0
!
interface Ethernet0/1
 ip address 192.168.111.63 255.255.255.0
 ipv6 address FE80::64:63:63 link-local
 ipv6 address 2001:64:63::63/64
!
interface Ethernet0/2
 ip address 192.168.118.63 255.255.255.0
 ipv6 address FE80::63:62:63 link-local
 ipv6 address 2001:63:62::63/64
!
router ospf 100
 router-id 63.63.63.63
 network 192.168.111.0 0.0.0.255 area 0
!
router bgp 65201
 bgp router-id 63.63.63.63
 bgp log-neighbor-changes
 network 63.63.63.63 mask 255.255.255.255
 neighbor 192.168.111.64 remote-as 65201
 neighbor 192.168.111.64 next-hop-self
 neighbor 192.168.118.62 remote-as 65202
```

-----

## 🔍 Validation

✅ After (on R64)
```bash
R64#sh ip bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *>i 60.60.60.60/32   192.168.110.60           0    100      0 i
 *>i 61.61.61.61/32   192.168.110.60           0    100      0 65200 i
 * i 62.62.62.62/32   192.168.118.62           0    100      0 65202 i
 *>i 63.63.63.63/32   192.168.111.63           0    100      0 i
 *>  64.64.64.64/32   0.0.0.0                  0         32768 i
```

-----

## 🖧 Topology

![Lab 1 Topology](../Diagrams/Screenshot_3.png)

-----
