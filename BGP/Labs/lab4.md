# 🔹 Lab 4 – BGP Configuration & Validation
## ⚙️ Configuration
### 🔀 **R58** 
```bash
router bgp 65204
 bgp router-id 58.58.58.58
 bgp log-neighbor-changes
 no bgp default ipv4-unicast
 neighbor 57.57.57.57 remote-as 65203
 neighbor 57.57.57.57 ebgp-multihop 2
 neighbor 57.57.57.57 update-source Loopback58
 neighbor 60.60.60.60 remote-as 65201
 neighbor 60.60.60.60 ebgp-multihop 2
 neighbor 60.60.60.60 update-source Loopback58
 neighbor 63.63.63.63 remote-as 65201
 neighbor 63.63.63.63 ebgp-multihop 2
 neighbor 63.63.63.63 update-source Loopback58
 neighbor 64.64.64.64 remote-as 65201
 neighbor 64.64.64.64 ebgp-multihop 2
 neighbor 64.64.64.64 update-source Loopback58
 !
 address-family ipv4
  network 58.58.58.58 mask 255.255.255.255
  neighbor 57.57.57.57 activate
  neighbor 60.60.60.60 activate
  neighbor 63.63.63.63 activate
  neighbor 63.63.63.63 route-map FILTRO-IN in
  neighbor 64.64.64.64 activate
  neighbor 64.64.64.64 route-map FILTRO-IN-2 in
 exit-address-family
!
ip route 57.57.57.57 255.255.255.255 192.168.116.57
ip route 60.60.60.60 255.255.255.255 192.168.113.60
ip route 63.63.63.63 255.255.255.255 192.168.115.63
ip route 64.64.64.64 255.255.255.255 192.168.114.64
!
route-map FILTRO-IN permit 10
 match ip address 1
 set weight 45000
!
route-map FILTRO-IN permit 90
!
route-map FILTRO-IN-2 permit 10
 match ip address 2
 set weight 46000
!
route-map FILTRO-IN-2 permit 90
!
access-list 1 permit 192.168.1.0 0.0.0.31
access-list 2 permit 192.168.1.32 0.0.0.31
access-list 2 permit 192.168.1.64 0.0.0.31
```

-----

## 🔍 Validation

✅ After 
```bash
R58#sh ip bgp
BGP table version is 13, local router ID is 58.58.58.58
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal,
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter,
              x best-external, a additional-path, c RIB-compressed,
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *   1.1.1.99/32      63.63.63.63                            0 65201 65200 i
 *                    57.57.57.57                            0 65203 65201 65200 i
 *                    64.64.64.64                            0 65201 65200 i
 *>                   60.60.60.60                            0 65201 65200 i
 *   11.11.11.11/32   63.63.63.63                            0 65201 65200 300 i
 *                    57.57.57.57                            0 65203 65201 65200 300 i
 *                    64.64.64.64                            0 65201 65200 300 i
 *>                   60.60.60.60                            0 65201 65200 300 i
 *   15.15.15.15/32   63.63.63.63                            0 65201 65202 500 i
 *                    57.57.57.57                            0 65203 65201 65202 500 i
 *                    64.64.64.64                            0 65201 65202 500 i
     Network          Next Hop            Metric LocPrf Weight Path
 *>                   60.60.60.60                            0 65201 65202 500 i
 *>  58.58.58.58/32   0.0.0.0                  0         32768 i
 r   60.60.60.60/32   63.63.63.63                            0 65201 i
 r                    57.57.57.57                            0 65203 65201 i
 r                    64.64.64.64                            0 65201 i
 r>                   60.60.60.60              0             0 65201 i
 *   61.61.61.61/32   63.63.63.63                            0 65201 65200 i
 *                    57.57.57.57                            0 65203 65201 65200 i
 *                    64.64.64.64                            0 65201 65200 i
 *>                   60.60.60.60                            0 65201 65200 i
 *   62.62.62.62/32   63.63.63.63                            0 65201 65202 i
 *                    57.57.57.57                            0 65203 65201 65202 i
 *                    64.64.64.64                            0 65201 65202 i
 *>                   60.60.60.60                            0 65201 65202 i
 r   63.63.63.63/32   63.63.63.63              0             0 65201 i
 r                    57.57.57.57                            0 65203 65201 i
 r                    64.64.64.64                            0 65201 i
 r>                   60.60.60.60                            0 65201 i
 r   64.64.64.64/32   63.63.63.63                            0 65201 i
 r                    57.57.57.57                            0 65203 65201 i
     Network          Next Hop            Metric LocPrf Weight Path
 r                    64.64.64.64              0             0 65201 i
 r>                   60.60.60.60                            0 65201 i
 *>  192.168.1.0/27   63.63.63.63                        45000 65201 65200 i                        <-- IMPORTANT!!!
 *                    57.57.57.57                            0 65203 65201 65200 i
 *                    64.64.64.64                            0 65201 65200 i
 *                    60.60.60.60                            0 65201 65200 i
 *   192.168.1.32/27  63.63.63.63                            0 65201 65200 i
 *                    57.57.57.57                            0 65203 65201 65200 i
 *>                   64.64.64.64                        46000 65201 65200 i                      <-- IMPORTANT!!!
 *                    60.60.60.60                            0 65201 65200 i
 *   192.168.1.64/27  63.63.63.63                            0 65201 65200 i
 *                    57.57.57.57                            0 65203 65201 65200 i
 *>                   64.64.64.64                        46000 65201 65200 i                      <-- IMPORTANT!!!
 *                    60.60.60.60                            0 65201 65200 i

```

-----

## 🖧 Topology

![Lab 4 Topology](../Diagrams/bgp2.png)

-----
