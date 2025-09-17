1️⃣ Configurar direcciones IP en PC1 y PC3 + default gateways.

2️⃣ Verificar conectividad básica (ping PC1 → PC3, loopback en D3, y gateway en R2).

3️⃣ Revisar tablas de enrutamiento OSPF en R1 y R3 → solo tienen sus rutas locales.

4️⃣ Configurar GRE over IPsec en R1 usando crypto maps (ISAKMP, pre-shared key, transform set en modo transport, ACL GRE, crypto map, aplicar en interfaz, crear tunnel).

5️⃣ Configurar GRE over IPsec en R3 usando IPsec profiles (ISAKMP, pre-shared key, transform set, perfil IPsec, aplicar en interfaz Tunnel1).

6️⃣ Habilitar OSPF en el túnel (R1 y R3) para que se propaguen rutas dinámicamente.

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

interface gi1
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
show ip ospf neighbor
show ip route ospf | begin Gateway
show ip route 172.16.0.0
```
