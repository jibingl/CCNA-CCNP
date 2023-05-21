# Border Gateway Protocol
IBGP (Interieor BGP) - AS to AS
EBGP (Extereior BGP) - Within an AS



```
AS100-BR1(config)# ip bgp 100
AS100-BR1(config-router)# bgp router-id 1.1.1.1
AS100-BR1(config-router)# neighbor 192.168.12.2 remote-as 200
AS100-BR1(config-router)# network 192.168.1.0/24

AS200-BR2(config)# ip bgp 200
AS200-BR2(config-router)# bgp router-id 2.2.2.2
AS200-BR2(config-router)# neighbor 192.168.12.1 remote-as 100
AS200-BR2(config-router)# network 192.168.2.0/24
```
