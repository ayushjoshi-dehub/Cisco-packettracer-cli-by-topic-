# Multi-user Connection with Switches and Routers (Peer Configuration)

## Topology Diagram

```
192.168.1.0/24                                              192.168.2.0/24
+-----+      +--------+   .1        .2   +--------+      +-----+
| PC0 |------| Switch |---| Router0 |------| Router1 |------| Switch |------| PC1 |
+-----+      +--------+   +--------+  192.168.5.0/24  +--------+     +-----+
                Peer 0  <========== simulated peer link ==========>  Peer 1
```

## Router 0

```
Router(config)#ip route 192.168.2.0 255.255.255.0 192.168.5.2
Router(config)#exit
```

## Router 1

```
Router(config)#ip route 192.168.1.0 255.255.255.0 192.168.5.1
Router(config)#exit
```

## Peer 0 — Simulated Connection Profile (Desktop App, not CLI)

```
Connection Type   : Outgoing
Peer Address      : Local host
Peer Port Number  : 38000
Peer Network Name : Peer 1
Password          : abc123
```

## Peer 1 — Simulated Connection Profile (Desktop App, not CLI)

```
Connection Type   : Incoming
Peer Address      : local/host
Peer Port Number  : 38000
Peer Network Name : Peer 1
Password          : abc123
```

## Verification

```
PC0> ping 192.168.2.100
Packets: Sent=4, Received=4, Lost=0
```

## Correction / Note

The two static routes must point to each other's real next-hop interface address, not the
same address on both ends — Router 0's route to 192.168.2.0/24 uses Router 1's link IP as
next-hop, and Router 1's route back uses Router 0's link IP, as corrected above. The peer
"connection profile" fields (Type/Address/Port/Password) are application-layer settings for
a simulated peer-to-peer desktop app in Packet Tracer, not IOS CLI commands — configure them
through the PC/server's Desktop > peer app GUI, not a router prompt.
