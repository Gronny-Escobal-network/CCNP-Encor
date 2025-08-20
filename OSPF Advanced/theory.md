# OSPF - Open Shortest Path First

## 📖 Conceptos Básicos
- OSPF es un protocolo de enrutamiento **IGP** de tipo **link-state**.  
- Utiliza el algoritmo **Dijkstra (SPF - Shortest Path First)**.  
- Soporta **VLSM** y **CIDR**.  
- Número de protocolo IP: **89**.  

## 🔑 Características
- Convergencia rápida.  
- Divide la red en **áreas** para mejorar escalabilidad.  
- Utiliza **coste (basado en ancho de banda)** como métrica.  
- Soporta **autenticación** (simple y MD5).  

## 🧩 Tipos de Routers en OSPF
- **IR (Internal Router)**: todos los interfaces en la misma área.  
- **ABR (Area Border Router)**: conecta dos o más áreas.  
- **ASBR (Autonomous System Border Router)**: redistribuye rutas externas.  
- **Backbone Router**: todo router con interfaz en el área 0.  

## 🗺️ Tipos de Áreas
- **Área 0 (Backbone)**: obligatoria.  
- **Área regular**: normal, con todas las LSAs.  
- **Stub area**: reduce LSAs externas.  
- **Totally stubby**: aún más resumida, solo default route.  
- **NSSA (Not-So-Stubby Area)**: permite rutas externas limitadas.  

---
