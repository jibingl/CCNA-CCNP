# Border Gateway Protocol
IBGP (Internal BGP) - AS to AS
EBGP (External BGP) - Within an AS

## Multiprotocols and Redistributing Routing Protocols


```
AS100-CBR(config)# ip bgp 100
AS100-CBR(config-router)# bgp router-id 1.1.1.1
AS100-CBR(config-router)# neighbor 192.168.12.2 remote-as 200
AS100-CBR(config-router)# network 192.168.1.0/24

AS200-CBR(config)# ip bgp 200
AS200-CBR(config-router)# bgp router-id 2.2.2.2
AS200-CBR(config-router)# neighbor 192.168.12.1 remote-as 100
AS200-CBR(config-router)# network 192.168.2.0/24

AS300-ISP-BR1(config)#
AS300-ISP-BR1(config-router)#

AS300-ISP-BR2(config)#
AS300-ISP-BR2(config-router)#
```
