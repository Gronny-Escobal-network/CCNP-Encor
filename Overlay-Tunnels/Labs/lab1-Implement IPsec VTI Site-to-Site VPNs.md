### R1 CONFIGURATION

```bash
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 3600
exit

crypto isakmp key cisco123 address 64.100.1.2

crypto ipsec transform-set VTI-VPN esp-aes 256 esp-sha256-hmac
 mode tunnel
exit

crypto ipsec profile VTI-PROFILE
 set transform-set VTI-VPN
exit

interface Tunnel1
 bandwidth 4000
 ip address 172.16.1.1 255.255.255.252
 ip mtu 1400
 tunnel source 64.100.0.2
 tunnel destination 64.100.1.2
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile VTI-PROFILE
exit

router ospf 123
 network 172.16.1.0 0.0.0.3 area 0
end

show interfaces tunnel 1
show crypto session
show ip route ospf | begin Gateway
show crypto ipsec sa | include encrypt|decrypt
show ip route 172.16.0.0
```


### R3 CONFIGURATION

```bash
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 3600
exit

crypto isakmp key cisco123 address 64.100.0.2

crypto ipsec transform-set VTI-VPN esp-aes 256 esp-sha256-hmac
 mode tunnel
exit

crypto ipsec profile VTI-PROFILE
 set transform-set VTI-VPN
exit

interface Tunnel1
 bandwidth 4000
 ip address 172.16.1.2 255.255.255.252
 ip mtu 1400
 tunnel source 64.100.1.2
 tunnel destination 64.100.0.2
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile VTI-PROFILE
exit

router ospf 123
 network 172.16.1.0 0.0.0.3 area 0
end

show interfaces tunnel 1
show crypto session
show ip route ospf | begin Gateway
show ip route 172.16.0.0
```
