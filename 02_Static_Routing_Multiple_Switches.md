# Static Routing — Network with Additional Switch (3 PCs, 2 Routers)

## Topology Diagram

```
192.168.1.0/24                              192.168.2.0/24          192.168.4.0/24
+------+   +--------+   .2        .1   +--------+   .1     +--------+   +------+
| PC0  |---| Switch |---| Router0 |-----| Router1 |---------| Switch |---| PC2  |
+------+   +--------+   +--------+      +--------+          +--------+   +------+
                                                                            |
                                                                          +------+
                                                                          | PC3  |
                                                                          +------+
```

## Router 0 Configuration

```
Router>enable
Router#configure terminal
Router(config)#ip route 192.168.2.0 255.255.255.0 192.168.1.2
Router(config)#ip route 192.168.4.0 255.255.255.0 192.168.1.2
Router(config)#exit
```

## Router 1 Configuration

```
Router>enable
Router#configure terminal
Router(config)#ip route 192.168.1.0 255.255.255.0 192.168.1.1
Router(config)#exit
```

## Verification

```
PC0> ping 192.168.2.100
PC0> ping 192.168.4.100
```

## Correction / Note

A router needs one `ip route` entry for every remote subnet it does not already have a
directly connected interface on — not just one line total. Double-check that the next-hop
IP in each `ip route` command is the address of the neighboring router's interface on the
shared link, not a LAN-side address.
