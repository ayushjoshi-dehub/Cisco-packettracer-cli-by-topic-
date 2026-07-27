# NAT Configuration

## Topology Diagram

```
 Inside                         Outside
+---------+  Gi0/1(inside)  Gi0/0(outside)   +---------+       Internet-side
| Server  |------------------| Router1 |------------------------| Router2 |----- Google/Gmail/
| .1.1    |                  +---------+  192.168.2.0/24        +---------+     TikTok/Facebook
+---------+                                                       192.168.1.0/24 (via NAT)

Static NAT: inside-local 192.168.1.1  <-->  inside-global 192.168.2.10
```

## Router 1 — NAT Router

```
Router>enable
Router#configure terminal
Router(config)#interface gigabitEthernet 0/1
Router(config-if)#ip nat inside
Router(config-if)#exit

Router(config)#interface gigabitEthernet 0/0
Router(config-if)#ip nat outside
Router(config-if)#exit

Router(config)#ip nat inside source static 192.168.1.1 192.168.2.10
Router(config)#exit
```

## Router 2 — Route to NAT Router

```
Router(config)#ip route 192.168.1.0 255.255.255.0 192.168.2.1
Router(config)#exit
```

## Correction / Note

`ip nat inside` and `ip nat outside` must be applied to two **different** interfaces on
the same router (the LAN-facing side and the WAN-facing side) — applying both to the same
interface, or forgetting one side, is the most common reason NAT silently fails in Packet
Tracer. Also confirm the static NAT's inside-local address (192.168.1.1) actually matches
the real server's configured IP.
