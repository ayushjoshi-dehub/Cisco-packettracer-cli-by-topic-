# OSPF — Topology 2 (R1 through R8)

## Topology Diagram

```
192.168.4.0/24     192.168.1.0/24     192.168.2.0/24      10.0.0.0/8       20.0.0.0/8
+-----+  +--------+   +--------+   +--------+   +--------+   +--------+   ...   +--------+
| PC0 |--| Router1 |---| Router2 |---| Router3 |---| Router4 |---| Router5 |----| Router8 |
+-----+  +--------+   +--------+   +--------+   +--------+   +--------+   ...   +--------+

All routers/interfaces reside in Area 0
```

## Router 1

```
Router>enable
Router#configure terminal
Router(config)#router ospf 1
Router(config-router)#network 192.168.4.0 0.0.0.255 area 0
Router(config-router)#network 192.168.1.0 0.0.0.255 area 0
Router(config-router)#exit
```

## Router 2

```
Router(config)#router ospf 1
Router(config-router)#network 192.168.1.0 0.0.0.255 area 0
Router(config-router)#network 192.168.2.0 0.0.0.255 area 0
Router(config-router)#exit
```

## Router 3

```
Router(config)#router ospf 1
Router(config-router)#network 10.0.0.0 0.255.255.255 area 0
Router(config-router)#network 192.168.2.0 0.0.0.255 area 0
Router(config-router)#exit
```

## Router 4

```
Router(config)#router ospf 1
Router(config-router)#network 20.0.0.0 0.255.255.255 area 0
Router(config-router)#network 10.0.0.0 0.255.255.255 area 0
Router(config-router)#exit
```

## Routers 5, 6, 7, 8

Follow the same pattern as Router 3 / Router 4 — one
`network <connected-subnet> <wildcard> area 0` line for every directly connected
interface on that router, then `exit`.

## Correction / Note

The original notes combined two different subnets into a single `network` statement on
one router (e.g. "192.168.4.0 & 192.168.1.0"). OSPF requires a **separate** `network`
command per subnet — two networks cannot share one line. This has been split out
correctly above.
