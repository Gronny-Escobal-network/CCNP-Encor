-----

## 📄 Local OSPF Filtering

In **OSPF (Open Shortest Path First)** networks, **route filtering** is a critical technique for controlling which prefixes get installed into a router's local routing table. Since OSPF is a link-state protocol, all routers within the same area must maintain an identical **LSDB (Link-State Database)**. For this reason, you can't remove routes directly from the LSDB, but you can prevent them from being installed in the **RIB (Routing Information Base)**, or local routing table.

### 📝 Using `distribute-list`

The `distribute-list` command is the primary tool for local route filtering in OSPF. It's important to understand its behavior:

  * **It filters the RIB, not the LSDB:** The `distribute-list` only stops information from the LSDB from being copied into the router's routing table. The route still exists in the link-state database, which is shared with all routers in the area.
  * **Behavior on an ABR (Area Border Router):**
      * It does not stop the conversion of Type 1 LSAs into Type 3 LSAs for other areas, because this process happens **before** the filter is applied.
      * It **does** prevent Type 3 LSAs from the backbone from being reinjected into non-backbone areas, because that process happens **after** the filter is applied.

-----

## 💻 Configuration

### **Z2** (Area 0)

This example shows how to filter the `192.168.200.200` route so it isn't installed in R2's routing table.

```bash
configure terminal
!
ip access-list standard FILTRO
 deny 192.168.200.200 0.0.0.0
 permit any
!
router ospf 100
 distribute-list FILTRO in
!
end
```

-----

### **Z11** (Area 1)

This router's configuration shows how to advertise the `192.168.200.200` network in OSPF Area 1.

```bash
Z11# show ip int brief | i 200
Loopback200        192.168.200.200 YES manual up up
!
router ospf 100
 router-id 11.11.11.11
 redistribute eigrp 100 metric 1 subnets
 network 10.0.64.0 0.0.15.255 area 1
 network 192.168.200.200 0.0.0.0 area 1  <-- This is the network that will be filtered on R2
 network 192.168.201.201 0.0.0.0 area 1
 network 192.168.202.202 0.0.0.0 area 1
 network 192.168.203.203 0.0.0.0 area 1
!
```

-----

## 🗺️ Topology

![Lab 1 Topology](../Diagrams/distribute_list.png)

-----
