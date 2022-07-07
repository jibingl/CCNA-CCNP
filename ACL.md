# Access Control List

### Syntax
```
R1(config)#ip access-list [standard] {numbered | named} {action} {source-ip-wildcard}
                                      1-99               permit   subnet
                                      1300-1999          deny     host
                                      <string>                    any
                                                                  
R1(config)#ip access-list [extended] {numbered | named} {action} {protocol} {source-ip-wildcard} {operator} {source-port} {dest-ip-wildcard} {operator} {dest-port} [established]
                                      100-199            permit   ip         subnet               eq         port-number   subnet             eq         port-number
                                      2000-2699          deny     tcp        host                 nq         port-name     host               nq         port-name
                                      <string>                    udp        any                  gt                       any                gt
                                                                  icmp                            lt                                          lt
                                                                  gre                             range                                       range
```
### Example
