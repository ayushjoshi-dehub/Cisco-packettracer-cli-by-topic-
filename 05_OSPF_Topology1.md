# OSPF — Topology 1 (R1, R2, R3)

## Topology Diagram

```
192.168.1.0/24         10.0.0.0/8   30.0.0.0/8         192.168.2.0/24
+-----+     +--------+  Se     Se   +--------+  Se     Se  +--------+     +-----+
| PC0 |-----| Router1 |----------------| Router2 |------------| Router3 |-----| PC1 |
+-----+     +--------+                +--------+             +--------+     +-----+
                  \                                                /
                   \______________________ 30.0.0.0/8 ____________/
                          (direct link Router1 <-> Router3)

All interfaces in Area 0
```

## Router 1

```
Router>enable
Router#configure terminal
Router(config)#router ospf 1
Router(config-router)#network 192.168.1.0 0.0.0.255 area 0
Router(config-router)#network 10.0.0.0 0.255.255.255 area 0
Router(config-router)#network 30.0.0.0 0.255.255.255 area 0
Router(config-router)#exit
```

## Router 2

```
Router>enable
Router#configure terminal
Router(config)#router ospf 1
Router(config-router)#network 10.0.0.0 0.255.255.255 area 0
Router(config-router)#network 20.0.0.0 0.255.255.255 area 0
Router(config-router)#exit
```

## Router 3

```
Router>enable
Router#configure terminal
Router(config)#router ospf 1
Router(config-router)#network 192.168.2.0 0.0.0.255 area 0
Router(config-router)#network 20.0.0.0 0.255.255.255 area 0
Router(config-router)#network 30.0.0.0 0.255.255.255 area 0
Router(config-router)#exit
```

## Correction / Note

The `network` wildcard mask must be the inverse of the actual subnet mask on that interface.
The /24 LANs correctly use `0.0.0.255`, but if the serial links are really /8 (mask
255.0.0.0) the wildcard `0.255.255.255` is right; if they are actually /30 point-to-point
links (mask 255.255.255.252), the wildcard should be `0.0.0.3` instead — verify the real
subnet mask on each serial interface in Packet Tracer before finalizing.
