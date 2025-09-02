# 🔹 Lab 2 – BGP Configuration & Validation
### Before (first sh ip bgp): 
For the networks 192.168.2.0/27 and 192.168.2.64/26 there were three valid entries each, with Next Hop 10.1.3.130 and 10.1.3.3 (neighbors on the interface toward R3) and 10.1.2.2 (neighbor toward R2). The *> (best) was already pointing to 10.1.2.2 (R2), but R3 was also advertising those networks (that’s why you see the lines with 10.1.3.3/10.1.3.130).

In this part of the lab, the goal is to manipulate BGP advertisements using an ACL + distribute-list, so that R3 only advertises to R1 the networks that really belong to ASN300, and not the ones from ASN200 that it was also propagating.

### After (after clear ip bgp * so): 
the entries from R3 for the 192.168.2.x networks disappeared; now only the route via 10.1.2.2 (R2) remains as valid and best (*>).
For the networks 192.168.3.0/27 and 192.168.3.64/26 the entries from R3 (10.1.3.3 / 10.1.3.130) are still appearing, because those are the own networks of AS300 and must continue to be advertised by R3.
## ⚙️ Configuration
### 🔀 **Q3** 
```bash
router bgp 3000
 bgp router-id 3.3.3.133
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 10.1.3.1 remote-as 1000
 neighbor 10.1.3.129 remote-as 1000
 neighbor 10.2.3.2 remote-as 5000
 !
 address-family ipv4
  network 192.168.3.0 mask 255.255.255.224
  network 192.168.3.64 mask 255.255.255.192
  neighbor 10.1.3.1 activate
  neighbor 10.1.3.1 distribute-list ALLOWED_TO_R1 out
  neighbor 10.1.3.129 activate
  neighbor 10.1.3.129 distribute-list ALLOWED_TO_R1 out
  neighbor 10.2.3.2 activate
 exit-address-family
!
ip access-list extended ALLOWED_TO_R1
 permit ip 192.168.3.0 0.0.0.31 any
 permit ip 192.168.3.64 0.0.0.63 any
```

-----

## 🔍 Validation

✅ Before 
```bash
Q1#sh ip bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *>  192.168.1.0/27   0.0.0.0                  0         32768 i
 *>  192.168.1.64/26  0.0.0.0                  0         32768 i
 *   192.168.2.0/27   10.1.3.130                             0 3000 5000 i         <---- I was lerning for this segment
 *                    10.1.3.3                               0 3000 5000 i         <---- I was lerning for this segment
 *>                   10.1.2.2                 0             0 5000 i              <---- I was lerning this segment as well
 *   192.168.2.64/26  10.1.3.130                             0 3000 5000 i
 *                    10.1.3.3                               0 3000 5000 i
 *>                   10.1.2.2                 0             0 5000 i
 *   192.168.3.0/27   10.1.3.130               0             0 3000 i
 *>                   10.1.3.3                 0             0 3000 i
 *                    10.1.2.2                               0 5000 3000 i
 *   192.168.3.64/26  10.1.3.130               0             0 3000 i
 *>                   10.1.3.3                 0             0 3000 i
 *                    10.1.2.2                               0 5000 3000 i

```
✅ After 
```bash
Q1#sh ip bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *>  192.168.1.0/27   0.0.0.0                  0         32768 i
 *>  192.168.1.64/26  0.0.0.0                  0         32768 i
 *>  192.168.2.0/27   10.1.2.2                 0             0 5000 i
 *>  192.168.2.64/26  10.1.2.2                 0             0 5000 i
 *   192.168.3.0/27   10.1.3.130               0             0 3000 i
 *>                   10.1.3.3                 0             0 3000 i
 *                    10.1.2.2                               0 5000 3000 i
 *   192.168.3.64/26  10.1.3.130               0             0 3000 i
 *>                   10.1.3.3                 0             0 3000 i
 *                    10.1.2.2                               0 5000 3000 i
```

-----

## 🖧 Topology

![Lab 7 Topology](../Diagrams/bgp7.png)

-----
