# Routing On A Stack

```
R1#config t
R1(config)#interface g0/1
R1(config-if)#no shutdown

R1(config-subif)#interface g0/1.10
R1(config-subif)#encapsulation dot1q 10
R1(config-subif)#ip address 192.168.10.1 255.255.255.0
R1(config-subif)#exit

R1(config)#interface g0/1.20
R1(config-subif)#encapsulation dot1q 20
R1(config-subif)#ip address 192.168.20.1 255.255.255.0
```
