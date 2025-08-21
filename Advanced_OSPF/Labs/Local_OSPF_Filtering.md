## Local OSPF Filtering

En algunos escenarios, es necesario remover rutas solo en routers específicos dentro de un área.  
OSPF es un protocolo de estado de enlace que requiere que todos los routers del mismo área mantengan una copia idéntica de la LSDB (Link-State Database).  

Una ruta puede existir en la LSDB de OSPF, pero puede ser evitada en la **RIB (Routing Information Base)** local utilizando un **Distribute List**.  

- El **distribute-list** filtra rutas solo en la tabla de enrutamiento local, no en la LSDB.  
- En un ABR, un distribute-list **no** evita que LSAs tipo 1 se conviertan en LSAs tipo 3 hacia otras áreas, ya que esa conversión ocurre antes de aplicar el filtro.  
- Sin embargo, sí evita que LSAs tipo 3 provenientes del backbone se reinyecten en áreas no-backbone, porque ese proceso ocurre después de aplicar el filtro.  
- Por esta razón, los distribute-list no son recomendados para filtrar prefijos entre áreas; existen técnicas más adecuadas para ello.  

El comando básico es:  

```bash
distribute-list {acl-number | acl-name | prefix prefix-list-name | route-map route-map-name} in
```
```bash
## Configuration

### R2 (Area 0 network)
  configure terminal
   ip access-list standard FILTRO
    deny 192.168.200.200 0.0.0.0
    permit any
   !
   router ospf 100
    distribute-list FILTRO in
  end


### R11 (Area 1 network)
  Z11# show ip int brief | i 200  
  Loopback200           192.168.200.200   YES manual up   up  
  
  router ospf 100  
   router-id 11.11.11.11  
   redistribute eigrp 100 metric 1 subnets  
   network 10.0.64.0 0.0.15.255 area 1  
   network 192.168.200.200 0.0.0.0 area 1    <-- IMPORTANTE
   network 192.168.201.201 0.0.0.0 area 1  
   network 192.168.202.202 0.0.0.0 area 1  
   network 192.168.203.203 0.0.0.0 area 1 
   

## Verification

### Before summarization (on Z4)


### After summarization (on Z4)
    


### Discard (summary) route on ABR (Q8)


Topology used:

![Lab 1 Topology](../Diagrams/OSPF_Interarea_Summarization.png)
