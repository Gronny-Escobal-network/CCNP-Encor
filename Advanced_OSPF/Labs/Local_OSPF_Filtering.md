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
