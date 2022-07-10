# ECMP (Equal Cost Multi-Path)
ECMP loads routing balancing for either dynamic or static routes.

Example 1:
```
R1(config)#show ip route
...
Gateway of last resort is not set
...
O      192.168.4.0/24 [110/3] via 10.0.13.2, 00:00:09, GigabitEthernet1/0
                      [110/3] via 10.0.12.2, 00:00:09, GigabitEthernet0/0
```
Example 2:
```
R1(config)#ip route 192.168.4.0 255.255.255.0 10.0.12.2
R1(config)#ip route 192.168.4.0 255.255.255.0 10.0.13.2
R1(config)#show ip route
...
Gateway of last resort is not set
...
S      192.168.4.0/24 [1/0] via 10.0.12.2
                      [1/0] via 10.0.13.2
```
