# VLAN Configuration (Switch 1 & Switch 2)

## Topology Diagram

```
        VLAN10=admin  VLAN20=Account  VLAN30=Exam
+-------+  Fa0/5  +---------+  Fa0/24 (trunk)  +---------+  Fa0/5  +-------+
| Admin1|---------| Switch1 |------------------| Switch2 |---------| Admin2|
+-------+         +---------+                  +---------+         +-------+
                   Fa0/1: Account1               Fa0/21-23: Account2
                   Fa0/11: Exam1                  Fa0/11: Exam2
```

## Switch 1

```
Switch>enable
Switch#configure terminal
Switch(config)#vlan 10
Switch(config-vlan)#name admin
Switch(config-vlan)#exit
Switch(config)#vlan 20
Switch(config-vlan)#name Account
Switch(config-vlan)#exit
Switch(config)#vlan 30
Switch(config-vlan)#name Exam
Switch(config-vlan)#exit

Switch(config)#interface fastEthernet 0/5
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit

Switch(config)#interface fastEthernet 0/1
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 20
Switch(config-if)#exit

Switch(config)#interface fastEthernet 0/11
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 30
Switch(config-if)#exit

Switch(config)#interface fastEthernet 0/24
Switch(config-if)#switchport mode trunk
Switch(config-if)#exit
```

## Switch 2

```
Switch>enable
Switch#configure terminal
Switch(config)#vlan 10
Switch(config-vlan)#name admin
Switch(config-vlan)#exit
Switch(config)#vlan 20
Switch(config-vlan)#name Account
Switch(config-vlan)#exit
Switch(config)#vlan 30
Switch(config-vlan)#name Exam
Switch(config-vlan)#exit

Switch(config)#interface fastEthernet 0/5
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 10
Switch(config-if)#exit

Switch(config)#interface fastEthernet 0/11
Switch(config-if)#switchport mode access
Switch(config-if)#switchport access vlan 30
Switch(config-if)#exit

Switch(config)#interface range fastEthernet 0/21-23
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 20
Switch(config-if-range)#exit

Switch(config)#interface fastEthernet 0/24
Switch(config-if)#switchport mode trunk
Switch(config-if)#exit
```

## Correction / Note

The `interface`/`interface range` commands need the full interface type name —
`fastEthernet 0/5`, not just `fa 0/5` shorthand (Packet Tracer does accept the `fa0/5`
abbreviation in most IOS versions, but writing it out avoids ambiguity). Also, a VLAN's
`name` command must be entered from VLAN config mode (`Switch(config-vlan)#`), not
global config mode.
