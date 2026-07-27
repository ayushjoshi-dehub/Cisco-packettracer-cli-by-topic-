# EIGRP (Dynamic Routing) — Router1 to Router4

## Topology Diagram

```
50.0.0.0/8            40.0.0.0/8            30.0.0.0/8            20.0.0.0/8            10.0.0.0/8
+-----+   Se +--------+ Se    Se +--------+ Se    Se +--------+ Se    Se +--------+ Se  +-----+
| PC0 |------| Router1 |---------| Router2 |---------| Router3 |---------| Router4 |-----| PC1 |
+-----+      +--------+          +--------+          +--------+          +--------+      +-----+

R1: network 50.0.0.0, 40.0.0.0    R2: network 40.0.0.0, 30.0.0.0
R3: network 30.0.0.0, 20.0.0.0    R4: network 20.0.0.0, 10.0.0.0
```

## Serial Interface Setup (DCE end example)

```
Router>enable
Router#configure terminal
Router(config)#interface serial0/1/0
Router(config-if)#ip address 20.0.0.1 255.0.0.0
Router(config-if)#clock rate 64000
Router(config-if)#no shutdown
Router(config-if)#exit
```

## Router 1 (PC0 side)

```
Router(config)#router eigrp 1
Router(config-router)#network 50.0.0.0
Router(config-router)#network 40.0.0.0
Router(config-router)#exit
```

## Router 2

```
Router(config)#router eigrp 1
Router(config-router)#network 40.0.0.0
Router(config-router)#network 30.0.0.0
Router(config-router)#exit
```

## Router 3

```
Router(config)#router eigrp 1
Router(config-router)#network 30.0.0.0
Router(config-router)#network 20.0.0.0
Router(config-router)#exit
```

## Router 4 (PC1 side)

```
Router(config)#router eigrp 1
Router(config-router)#network 20.0.0.0
Router(config-router)#network 10.0.0.0
Router(config-router)#exit
```

## Correction / Note

EIGRP autonomous-system numbers must match on every router in the domain (used `1`
consistently, which is correct). Only the DCE-side interface of a serial link needs
`clock rate` — setting it on the DTE side causes a config error in Packet Tracer. Use
`show controllers serial0/x/x` to confirm which end is DCE.
