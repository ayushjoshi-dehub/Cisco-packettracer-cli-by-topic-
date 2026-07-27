# Static Routing — Two Routers

## Topology Diagram

```
   192.168.1.0/24                                   192.168.3.0/24
+--------+       +--------+   .1        .2   +--------+       +--------+
|  PC0   |-------| Switch |---| Router0 |-----| Router1 |------| Switch |-------|  PC1   |
+--------+       +--------+   +--------+  192.168.2.0/24  +--------+   +--------+       +--------+
   .2                          Gi0/0=.1       Gi0/1=.1  .2=Gi0/1     Gi0/0=.1
                                                                                 
Router0 Gi0/0: 192.168.1.1/24        Router0 Gi0/1: 192.168.2.1/24
Router1 Gi0/0: 192.168.3.1/24        Router1 Gi0/1: 192.168.2.2/24
```

## Router 0 Configuration

```
Router>enable
Router#configure terminal
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 192.168.1.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.2.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#ip route 192.168.3.0 255.255.255.0 192.168.2.2
Router(config)#exit
```

## Router 1 Configuration

```
Router>enable
Router#configure terminal
Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip address 192.168.3.1 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip address 192.168.2.2 255.255.255.0
Router(config-if)#no shutdown
Router(config-if)#exit
Router(config)#ip route 192.168.1.0 255.255.255.0 192.168.2.1
Router(config)#exit
```

## Verification

```
PC0> ping 192.168.3.2
PC1> ping 192.168.1.2
```

## Correction / Note

Interface IP addressing and `no shutdown` must be configured on the router-to-router link
before the static route becomes active — the original notes only listed the `ip route` line.
Without a reachable next-hop, Packet Tracer shows the route as inactive.
