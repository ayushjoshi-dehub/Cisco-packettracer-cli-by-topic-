# RIP v2 (Dynamic Routing)

## Topology Diagram

```
40.0.0.0/8                  10.0.0.0/8   20.0.0.0/8                  30.0.0.0/8
+-----+       +--------+    Se     Se    +--------+       +-----+
| PC0 |-------| Router0 |------------------| Router1 |-------| PC1 |
+-----+       +--------+                  +--------+       +-----+

Router0: network 40.0.0.0, 10.0.0.0     Router1: network 20.0.0.0, 30.0.0.0
```

## Router 0 (LAN 40.0.0.0, link 10.0.0.0)

```
Router>enable
Router#configure terminal
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#network 40.0.0.0
Router(config-router)#network 10.0.0.0
Router(config-router)#exit
```

## Router 1 (LAN 30.0.0.0, link 20.0.0.0)

```
Router>enable
Router#configure terminal
Router(config)#router rip
Router(config-router)#version 2
Router(config-router)#network 20.0.0.0
Router(config-router)#network 30.0.0.0
Router(config-router)#exit
```

## Verification

```
PC0> ping 192.168.2.100
Packets: Sent=4, Received=4, Lost=0 (0% loss)
```

## Correction / Note

RIP `network` statements take the classful network address (e.g. `10.0.0.0`), never a
subnet mask or wildcard — this matches the original notes. Remember RIPv2 still has a
15-hop-count limit, which is fine for small labs like this.
