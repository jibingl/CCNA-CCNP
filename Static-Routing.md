# Static Routing

## Syntax
```
Router(config)#ip route <dest-address> <dest-mask> {<next-hop-IP | exit-interface>} [distance-metric<1-255>]
```

## Floating Static Routes
By chaning the AD of a static route, make it less preferred than routes learned by a dynamic routing protocol to the same destination (make sure the AD is higher than the dynamic routing protocol's AD)  
Example:
```
R1(config)#ip route 10.0.0.0 255.0.0.0 10.0.13.2 100
R1(config)#show ip route
...
Gateway of last resort is not set
...
S      10.0.0.0/8 [100/0] via 10.0.13.2
...
D      10.0.24.0/30 [90/3072] via 10.0.12.2, 00:06:35, GigabitEthernet0/0
```
