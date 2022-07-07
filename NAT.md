# NAT (Network Address Translation)
## Static NAT
_one private IP <--> one public IP_ 
```
192.168.1.10--[=]----------------------------g1/0---(+)---g0/0----------------------{ISP}----8.8.8.8
                                              .1     R1    .2       122.11.0/30   .1 
                                                
                                         Translations Table
.-Pro---Inside global----------Inside local----------|--Outside local--------Outside global------.
| udp   122.11.11.2:61116      192.168.1.10:61116    |  8.8.8.8:53           8.8.8.8:53          |
`---------------------static/permanent---------------|--------------dynamic/temporary------------`
```
Static NAT cmd
```
R1(config)#int g0/1
R1(config-if)#ip nat inside
R1(config-if)#int g0/0
R1(config-if)#ip nat outside
R1(config-if)#exit

R1(config)#ip nat inside source static 192.168.1.10 122.11.11.2
//Syntax: ip nat inside source static <inside-local> <inside-global>
```
## Dynamic NAT
_one or more private subnets <--> multiple public IPs_
```
192.168.1.0/24--[=]--------------------------g1/0---(+)---g0/0----------------------{ISP}----{Internet}
                                              .1     R1    .2       122.11.0/29   .1 
 
                                         Translations Table
.-Pro---Inside global----------Inside local----------|--Outside local--------Outside global------.
| icmp  122.11.11.2:3          192.168.1.10:3        |  8.8.8.8:3            8.8.8.8:3           |
| udp   122.11.11.3:58611      192.168.1.22:58611    |  1.1.1.1:53           1.1.1.1:53          |
| ---   122.11.11.4            192.168.1.100         |  ---                  ---                 |
| tcp   122.11.11.5:59112      192.168.1.88:59112    |  142.250.190.46:443   142.250.190.46:443  |
| tcp   122.11.11.6:61616      192.168.1.102:61616   |  140.82.113.4:443     140.82.113.4:443    |
`---------------dynamic(24h)/permanent---------------|--------------dynamic/temporary------------`
```
Dynamic NAT cmd
```
//...Omitc configuration of inside and outside interfaces
R1(config)#access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)#ip nat pool POOL-1 122.11.11.2 122.11.11.6 netmask 255.255.255.248        //Or type 'prefix-length 29'

R1(config)#ip nat inside source list 1 pool POOL-1
//Syntax: ip nat inside source <acl> <nat-pool>
```
## PAT (aka NAT overload)
_one or more private subnets <--> one or more public IPs_
```
192.168.1.0/24--[=]--------------------------g1/0---(+)---g0/0----------------------{ISP}----{Internet}
                                              .1     R1    .2       122.11.0/29   .1 
 
                                         Translations Table
.-Pro---Inside global----------Inside local----------|--Outside local--------Outside global------.
| udp   122.11.11.2:65535      192.168.1.10:65535    |  8.8.8.8:53           8.8.8.8:53          |
| udp   122.11.11.2:54321      192.168.1.102:54321   |  8.8.8.8:53           8.8.8.8:53          |
| udp   122.11.11.2:54322      192.168.1.100:54321   |  8.8.8.8:53           8.8.8.8:53          |
| tcp   122.11.11.3:59112      192.168.1.88:59112    |  142.250.190.46:443   142.250.190.46:443  |
| tcp   122.11.11.3:61616      192.168.1.22:61616    |  140.82.113.4:443     140.82.113.4:443    |
`---------------dynamic(24h)/permanent---------------|--------------dynamic/temporary------------`
```
PAT cmd
```
//...Omitc configuration of inside and outside interfaces
R1(config)#access-list 1 permit 192.168.1.0 0.0.0.255
R1(config)#ip nat pool POOL-1 122.11.11.2 122.11.11.3 prefix-length 29        //Prefix is not really impact the IP range

R1(config)#ip nat inside source list 1 pool POOL-1 overload
//Syntax: ip nat inside source <acl> <nat-pool> overload

R1(config)#ip nat inside source list 1 interface g0/0 overload                //Use single public IP for PAT
```
### Other NAT cmds
```
clear ip nat translation *
show ip nat translations
show ip nat statistics
```
