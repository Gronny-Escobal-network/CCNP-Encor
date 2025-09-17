### R1 CONFIGURATION
```bash
interface tunnel 0
 ip address 100.100.100.1 255.255.255.252
 bandwidth 4000
 ip mtu 1400
 tunnel source loopback 0
 tunnel destination 192.168.3.1
exit

router ospf 4
 router-id 1.1.1.4
 network 100.100.100.0 0.0.0.3 area 0
 network 172.16.1.0 0.0.0.255 area 1
exit

interface tunnel 1
 ipv6 address 2001:db8:ffff::1/64
 bandwidth 4000
 tunnel source loopback 0
 tunnel destination 2001:db8:acad:3::1
 tunnel mode gre ipv6
exit

ipv6 router ospf 6
 router-id 1.1.1.6
exit

interface tunnel 1
 ipv6 ospf 6 area 0
exit

interface loopback 1
 ipv6 ospf 6 area 1
exit
```
### R3 CONFIGURATION
```bash
interface tunnel 0
 ip address 100.100.100.2 255.255.255.252
 bandwidth 4000
 ip mtu 1400
 tunnel source loopback 0
 tunnel destination 192.168.1.1
exit

router ospf 4
 router-id 3.3.3.4
 network 100.100.100.0 0.0.0.3 area 0
 network 172.16.3.0 0.0.0.255 area 1
exit

interface tunnel 1
 ipv6 address 2001:db8:ffff::2/64
 bandwidth 4000
 tunnel source loopback 0
 tunnel destination 2001:db8:acad:1::1
 tunnel mode gre ipv6
exit

ipv6 router ospf 6
 router-id 3.3.3.6
exit

interface tunnel 1
 ipv6 ospf 6 area 0
exit

interface loopback 1
 ipv6 ospf 6 area 1
exit
```
