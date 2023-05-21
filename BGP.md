# Border Gateway Protocol
BGP is an exterior gateway protocol designed to exchange routing and reachability information among ASes on the Internet.  
- IBGP (Internal BGP) - AS to AS
- EBGP (External BGP) - Within an AS

Each AS is identified by an ASN that is either public or private. A public ASN is globally unique and can be advertised across the Internet; however, a private ASN is not globally unique and should not be advertised to external networks. Private ASNs range from 64512 to 65534, and from 4,200,000,000 to 4,294,967,294. All other ASNs are public and available for use on the Internet except for a few reserved numbers.  

A public ASN is required only when an AS is originating routes that are visible on the Internet. However, a private ASN should be used when an AS is only exchanging routes via BGP with a single Internet Service Provider (ISP).  
**E.G.** The AS of the customer is assigned a private A SN (64512) since the customer is connected to one ISP via BGP. The ISP has a public ASN (100) since it originates the routes that are visible on the Internet. 

## Multiprotocols and Redistributing Routing Protocols

## AS_PATH
The AS_PATH attribute is a list of all ASes that a specific route passes through to reach a specified network. When a router is advertising a BGP route, the AS_PATH attribute is first created empty. Each time the route is advertised from one AS to another, the AS_PATH attribute is modified to prepend the ASN of the router that advertised the route.

Routers use the AS_PATH attribute to detect and prevent loops. For example, a router drops any route in which its own ASN is part of the AS_PATH attribute.

Private ASNs are not globally unique, hence, they cannot be leaked to the Internet. To achieve this goal, routers must strip the private ASNs from the AS_PATH attribute list before the routes are advertised to the Internet.




```
AS100-CBR(config)# ip bgp 100                                                   //ASN is 100
AS100-CBR(config-router)# bgp router-id 1.1.1.1
AS100-CBR(config-router)# neighbor 192.168.12.2 remote-as 200                   //Neighbor ASN is 200
AS100-CBR(config-router)# network 192.168.1.0/24                                //The netowrks are advertised out via BGP

AS200-CBR(config)# access-list 10 permit 192.168.2.0 255.255.255.0 
AS200-CBR(config-router)# ip bgp 200
AS200-CBR(config-router)# bgp router-id 2.2.2.2
AS200-CBR(config-router)# neighbor 192.168.12.1 remote-as 100
AS200-CBR(config-router)# network 192.168.2.0/24
AS200-CBR(config-router)# neighbor 192.168.12.1 password cisco                   //BGP peers authentication
AS200-CBR(config-router)# neighbor 192.168.12.1 distribute-list 1 out            //Filter routing advertisement
AS200-CBR(config-router)# neighbor 192.168.12.1 default-orginate                 //Advertise default routes out via BGP

AS300-ISP-BR1(config)#
AS300-ISP-BR1(config-router)#

AS300-ISP-BR2(config)#
AS300-ISP-BR2(config-router)# neighbor 192.168.23.2 remove-private-AS            //Remove private ASN information from AS_PATH attributes that received from a neighbor
AS300-ISP-BR2(config)# bgp as-path access-list 1 deny ^100$                      //Matches only AS_PATHs that are sourced from AS 100
AS300-ISP-BR2(config)# bgp as-path access-list 1 permit .*                       //Matches any AS_PATH that has not been denied by the previous ACL statement
AS300-ISP-BR2(config-router)# neighbor 192.168.23.2 filter-list 1 out
```

