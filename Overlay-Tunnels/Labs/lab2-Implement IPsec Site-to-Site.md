✅Step 1: Configurar la política ISAKMP (Phase 1) en R1 y R3.

✅Step 2: Configurar la pre-shared key entre R1 y R3.

✅Step 3: Crear el transform-set (Phase 2) para definir cifrado y hash.

✅Step 4: Ajustar el lifetime de la Security Association.

✅Step 5: Configurar el ACL de tráfico interesante (qué redes viajan por el túnel).

✅Step 6: Crear y aplicar el crypto map con peer, transform-set y ACL.

✅Step 7: Aplicar el crypto map a la interfaz de salida (g0/0/0 en ambos routers).

✅Step 8: Verificar el túnel con comandos de ISAKMP, IPsec y OSPF.
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

crypto ipsec transform-set S2S-VPN esp-aes 256 esp-sha256-hmac
exit

crypto ipsec security-association lifetime seconds 1800

ip access-list extended S2S-VPN-ACL
 permit ip 10.10.0.0 0.0.3.255 10.10.4.0 0.0.3.255
 permit ip 10.10.0.0 0.0.3.255 10.10.16.0 0.0.7.255
exit

crypto map S2S-CMAP 10 ipsec-isakmp
 match address S2S-VPN-ACL
 set peer 64.100.1.2
 set pfs group14
 set transform-set S2S-VPN
 set security-association lifetime seconds 900
exit

interface g0/0/0
 crypto map S2S-CMAP
end
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

crypto ipsec transform-set S2S-VPN esp-aes 256 esp-sha256-hmac
exit

crypto ipsec security-association lifetime seconds 1800

ip access-list extended S2S-VPN-ACL
 permit ip 10.10.4.0 0.0.3.255 10.10.0.0 0.0.3.255
 permit ip 10.10.16.0 0.0.7.255 10.10.0.0 0.0.3.255
exit

crypto map S2S-CMAP 10 ipsec-isakmp
 match address S2S-VPN-ACL
 set peer 64.100.0.2
 set pfs group14
 set transform-set S2S-VPN
 set security-association lifetime seconds 900
exit

interface g0/0/0
 crypto map S2S-CMAP
end
```

### VERIFICATION
```bash
show ip route ospf
show crypto isakmp policy
show crypto ipsec transform-set
show crypto map
show crypto isakmp sa
show crypto ipsec sa
```
