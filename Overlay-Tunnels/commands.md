## R1
```bash
hostname R1
no ip domain lookup
line con 0
 logging sync
 exec-time 0 0
 exit
banner motd # This is R1, Implement GRE over IPsec Site-to-Site VPNs #
interface gi1
 description Connection to R2
 ip add 64.100.0.2 255.255.255.252
 no shut
 exit
interface Gi2
 description Connection to D1
 ip address 10.10.0.1 255.255.255.252
 no shut
 exit
router ospf 123
 router-id 1.1.1.1
 auto-cost reference-bandwidth 1000
 network 10.10.0.0 0.0.0.3 area 0
 default-information originate
exit
ip route 0.0.0.0 0.0.0.0 64.100.0.1
```

## R2
```bash
hostname R2
no ip domain lookup
line con 0
 logging sync
 exec-time 0 0
 exit
banner motd # This is R2, Implement GRE over IPsec Site-to-Site VPNs #
interface gi2
 description Connection to R1
 ip add 64.100.0.1 255.255.255.252
 no shut
 exit
interface Gi3
 description Connection to R3
 ip address 64.100.1.1 255.255.255.252
 no shut
 exit
int lo0
 description Internet simulated address
 ip add 209.165.200.225 255.255.255.224
 exit
ip route 0.0.0.0 0.0.0.0 Loopback0
ip route 10.10.0.0 255.255.252.0 64.100.0.2
ip route 10.10.4.0 255.255.252.0 64.100.1.2
ip route 10.10.16.0 255.255.248.0 64.100.1.2
```

## R3
```bash
hostname R3
no ip domain lookup
line con 0
 logging sync
 exec-time 0 0
 exit
banner motd # This is R3, Implement GRE over IPsec Site-to-Site VPNs #
interface gi3
 description Connection to R2
 ip add 64.100.1.2 255.255.255.252
 no shut
 exit
interface Gi1
 description Connection to D3
 ip address 10.10.4.1 255.255.255.252
 no shut
 exit
ip route 0.0.0.0 0.0.0.0 64.100.1.1
router ospf 123
 router-id 3.3.3.1
 auto-cost reference-bandwidth 1000
 network 10.10.4.0 0.0.0.3 area 0
 default-information originate
exit
```
