### R1 CONFIGURATION (GRE over IPsec using Crypto Map)
```bash
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 3600
exit

crypto isakmp key cisco123 address 64.100.1.2

crypto ipsec transform-set GRE-VPN esp-aes 256 esp-sha256-hmac
 mode transport
exit

ip access-list extended GRE-VPN-ACL
 permit gre host 64.100.0.2 host 64.100.1.2
exit

crypto map GRE-CMAP 10 ipsec-isakmp
 match address GRE-VPN-ACL
 set transform-set GRE-VPN
 set peer 64.100.1.2
exit

interface g0/0/0
 crypto map GRE-CMAP
exit

interface Tunnel1
 bandwidth 4000
 ip address 172.16.1.1 255.255.255.252
 ip mtu 1400
 tunnel source 64.100.0.2
 tunnel destination 64.100.1.2
exit

router ospf 123
 network 172.16.1.0 0.0.0.3 area 0
end

show interfaces tunnel 1
show crypto session
show crypto ipsec sa | include encrypt|decrypt
show ip ospf interface brief
show ip ospf neighbor
show ip route ospf | begin Gateway
show ip route 172.16.0.0
```

### R3 CONFIGURATION (GRE over IPsec using Tunnel IPsec Profile)
```bash
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 3600
exit

crypto isakmp key cisco123 address 64.100.0.2

crypto ipsec transform-set GRE-VPN esp-aes 256 esp-sha256-hmac
 mode transport
exit

crypto ipsec profile GRE-PROFILE
 set transform-set GRE-VPN
exit

interface Tunnel1
 bandwidth 4000
 ip address 172.16.1.2 255.255.255.252
 ip mtu 1400
 tunnel source 64.100.1.2
 tunnel destination 64.100.0.2
 tunnel protection ipsec profile GRE-PROFILE
exit

router ospf 123
 network 172.16.1.0 0.0.0.3 area 0
end

show interfaces tunnel 1
show crypto session
show ip ospf interface brief
```
show ip ospf neighbor
show ip route ospf | begin Gateway
show ip route 172.16.0.0
