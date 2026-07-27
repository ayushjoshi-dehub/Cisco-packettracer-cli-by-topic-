# Access Control List (ACL)

## Topology Diagram

```
192.168.1.0/24                 192.168.3.0/24                192.168.2.0/24
+-----+       +--------+  .1        .2  +--------+  .1          Gi0/0(ACL out)
| PC0 |-------| Router0 |------------------| Router1 |----------------------- PC .100 / .200
+-----+       +--------+                  +--------+
                Gi0/0: ACL 1 applied "out"     Gi0/0: ACL 1 applied "out"
Router0: deny host 192.168.2.200, permit any
Router1: deny host 192.168.2.100, permit any
```

## Router 0

```
Router(config)#ip route 192.168.2.0 255.255.255.0 192.168.3.1

Router(config)#access-list 1 deny host 192.168.2.200
Router(config)#access-list 1 permit any

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip access-group 1 out
Router(config-if)#exit
```

## Router 1

```
Router(config)#ip route 192.168.1.0 255.255.255.0 192.168.5.2

Router(config)#access-list 1 deny host 192.168.2.100
Router(config)#access-list 1 permit any

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip access-group 1 out
Router(config-if)#exit
```

## Verification

```
PC0> ping 192.168.2.100      ! permitted -> 4 sent, 4 received, 0% loss
PC0> ping 192.168.2.200      ! denied    -> 4 sent, 0 received, 100% loss
```

## Correction / Note

For a single host, standard ACLs need the keyword `host` before the address
(`deny host 192.168.2.200`), not the bare IP alone — without `host` or a wildcard mask,
IOS/Packet Tracer rejects the line or misinterprets it. Also double-check whether the ACL
should be applied `in` (closest to source) or `out` (closest to destination) on the
correct interface — `in` on the LAN-facing interface is usually better practice than `out`
on the WAN interface.
