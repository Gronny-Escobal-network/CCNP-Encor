# 🔹 Lab 1 – OSPF Configuration & Validation
## ⚙️ Configuration
### 🔀 **Q242** (Area 12)
```bash
router ospf 1
 router-id 192.168.1.1
 network 172.16.1.0 0.0.0.255 area 12
 network 172.16.2.0 0.0.0.255 area 12
 network 172.16.3.0 0.0.0.255 area 12
 network 10.12.0.0 0.0.255.255 area 12
```
### 🔀 **Q8** (ABR)

```bash
router ospf 1
 router-id 192.168.2.2
 area 12 range 172.16.0.0 255.255.0.0 cost 45
 network 10.12.0.0 0.0.255.255 area 12
 network 10.23.0.0 0.0.255.255 area 0
```
### 🔀 **Z4** (Area 0)

```bash
router ospf 1
 router-id 192.168.3.3
 network 10.23.0.0 0.0.255.255 area 0
```
-----

## 🔍 Validation

✅ Before summarization (on Z4)
```bash
Z4# show ip route ospf
  O IA 172.16.1.0/24 [110/3] via 10.23.1.2
  O IA 172.16.2.0/24 [110/3] via 10.23.1.2
  O IA 172.16.3.0/24 [110/3] via 10.23.1.2
```
✅ After summarization (on Z4)
```bash
Z4# show ip route ospf
  O IA 172.16.0.0/16 [110/46] via 10.23.1.2
```
✅ Discard (summary) route on ABR (Q8)
```bash
Q8# show ip route ospf
  O 172.16.0.0/16 is a summary, Null0
```

-----

## 🖧 Topology

![Lab 1 Topology](../Diagrams/OSPF_Interarea_Summarization.png)

-----
