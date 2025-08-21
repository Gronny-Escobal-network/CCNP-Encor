| **Task** | **Command Syntax** |
|----------|---------------------|
| Initialize the OSPF process | `router ospf process-id` |
| Summarize routes crossing an OSPF ABR | `area area-id range network subnet-mask [advertise \| not-advertise] [cost metric]` |
| Filter routes crossing an OSPF ABR | `area area-id filter-list prefix prefix-list-name {in \| out}` |
| Filter OSPF routes from entering the RIB | `distribute-list {acl-number \| acl-name \| prefix prefix-list-name \| route-map route-map-name} in` |
| Display the LSAs in the LSDB | `show ip ospf database [router \| network \| summary]` |

