# 🔹 Lab 9 – BGP Configuration & Validation

In this step, you configured on R1 an AS-PATH ACL to control which routes are advertised to R2 (ASN5000). Before applying the filter, R1 was sending not only its own networks (ASN1000), but also prefixes learned from other ASes (like ASN3000), which made ASN1000 act as a transit AS, an undesirable scenario. To prevent this, an AS-PATH access list was created with ^$, which means “only locally originated routes” (no other AS in the path). Then, this filter was applied in the out direction toward the neighbor R2. With this, R1 only advertises to R2 the networks that actually belong to ASN1000, discarding external routes. When checking on R2, the BGP table shows that now all prefixes with ASN1000 originate only in ASN1000, confirming that the filter works correctly.

## ⚙️ Configuration
### 🔀 **Q1** 
```bash
router bgp 1000
 bgp router-id 1.1.1.100
 bgp log-neighbor-changes
 neighbor 10.1.2.2 remote-as 5000
 neighbor 10.1.3.3 remote-as 3000
 neighbor 10.1.3.130 remote-as 3000
 !
 address-family ipv4
  network 192.168.1.0 mask 255.255.255.224
  network 192.168.1.64 mask 255.255.255.192
  neighbor 10.1.2.2 activate
  neighbor 10.1.2.2 prefix-list ALLOWED_FROM_R2 in
  neighbor 10.1.2.2 filter-list 1 out                  ✅
  neighbor 10.1.3.3 activate
  neighbor 10.1.3.130 activate
 exit-address-family
!
ip forward-protocol nd
!
ip as-path access-list 1 permit ^$                      ✅
```

-----

## 🔍 Validation

👉 Before 
```bash
Q2#sh ip bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *   192.168.1.0/27   10.2.3.3                               0 3000 1000 i
 *>                   10.1.2.1                 0             0 1000 i
 *   192.168.1.64/26  10.2.3.3                               0 3000 1000 i
 *>                   10.1.2.1                 0             0 1000 i
 *>  192.168.2.0/27   0.0.0.0                  0         32768 i
 *>  192.168.2.64/26  0.0.0.0                  0         32768 i
 *   192.168.3.0/27   10.1.2.1                               0 1000 3000 i
 *>                   10.2.3.3                 0             0 3000 i
 *   192.168.3.64/26  10.1.2.1                               0 1000 3000 i
 *>                   10.2.3.3                 0             0 3000 i

```
👉 After 
```bash
Q2#sh ip bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *   192.168.1.0/27   10.2.3.3                               0 3000 1000 i
 *>                   10.1.2.1                 0             0 1000 i
 *   192.168.1.64/26  10.2.3.3                               0 3000 1000 i
 *>                   10.1.2.1                 0             0 1000 i
 *>  192.168.2.0/27   0.0.0.0                  0         32768 i
 *>  192.168.2.64/26  0.0.0.0                  0         32768 i
 *>  192.168.3.0/27   10.2.3.3                 0             0 3000 i
 *>  192.168.3.64/26  10.2.3.3                 0             0 3000 i

```

-----

## 🖧 Topology

![Lab 9 Topology](../Diagrams/bgp7.png)

-----
