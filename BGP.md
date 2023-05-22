# Border Gateway Protocol
BGP is an exterior gateway protocol designed to exchange routing and reachability information among ASes on the Internet.  
- IBGP (Internal BGP) - AS to AS
- EBGP (External BGP) - Within an AS

Each AS is identified by an ASN that is either public or private. A public ASN is globally unique and can be advertised across the Internet; however, a private ASN is not globally unique and should not be advertised to external networks. Private ASNs range from 64512 to 65534, and from 4,200,000,000 to 4,294,967,294. All other ASNs are public and available for use on the Internet except for a few reserved numbers.  

A public ASN is required only when an AS is originating routes that are visible on the Internet. However, a private ASN should be used when an AS is only exchanging routes via BGP with a single Internet Service Provider (ISP).  
**E.G.** The AS of the customer is assigned a private A SN (64512) since the customer is connected to one ISP via BGP. The ISP has a public ASN (100) since it originates the routes that are visible on the Internet. 

Attributes | CLI |
-----------|----------
AS_PATH    | `(config)# access-list 10 permit 192.168.2.0 255.255.255.0` <br> `(config)# bgp as-path access-list 1 deny ^100$` <br> `(config)# bgp as-path access-list 1 permit .*` <br> `(config-router)# neighbor 192.168.23.2 remove-private-AS`
LOCAL_PREF | `(config)# route-map primary_outbound permit 10` <br> `(config-route-map)# set local-preference 150`
MED        | `(config)# route-map primary_med_inbound permit 10` <br> `(config-route-map)# set metric 50`
NEXT HOP   | 

## AS_PATH
The AS_PATH attribute is a list of all ASes that a specific route passes through to reach a specified network. When a router is advertising a BGP route, the AS_PATH attribute is first created empty. Each time the route is advertised from one AS to another, the AS_PATH attribute is modified to prepend the ASN of the router that advertised the route.

Routers use the AS_PATH attribute to detect and prevent loops. For example, a router drops any route in which its own ASN is part of the AS_PATH attribute.

Private ASNs are not globally unique, hence, they cannot be leaked to the Internet. To achieve this goal, routers must strip the private ASNs from the AS_PATH attribute list before the routes are advertised to the Internet.

## Local Preference (LOCAL_PREF)
A router sending IBGP update packets will include the LOCAL_PREF attribute. This attribute indicates the degree of preference for one BGP route over the others when an AS has multiple routes to another AS. The LOCAL_PREF attribute is always advertised to IBGP neighbors and never advertised to EBGP peers, hence, it is exchanged only among routers within the same AS. The BGP route with the highest LOCAL_PREF value is preferred.
```
(config)# route-map primary_outbound permit 10
(config-route-map)# set local-preference 150
(config-route-map)# exit
(config)# router ospf 200
(config-router)# neighbor 192.168.12.1 route-map primary_outbound in

(config)# route-map secondary_outbound permit 10
(config-route-map)# set local-preference 125
(config-route-map)# exit
(config)# router ospf 200
(config-router)# neighbor 192.168.13.1 route-map secondary_outbound in
```

## Multi Exit Discriminator (MED)
The MED attribute indicates to external neighbors the preferred path into an AS if there are multiple entry points into the same AS. The BGP route with the lowest MED value is preferred.  
The MED is sent to EBGP peers; those peer routers propagate the MED attribute within their AS, but do not pass it on to the next AS.
```
(config)# route-map primary_med_inbound permit 10
(config-route-map)# set metric 50
(config-route-map)# exit
(config)# router ospf 200
(config-router)# neighbor 192.168.12.1 route-map primary_med_inbound out

(config)# route-map secondary_med_inbound permit 10
(config-route-map)# set metric 75
(config-route-map)# exit
(config)# router ospf 200
(config-router)# neighbor 192.168.13.1 route-map secondary__med_inbound out
```

## IBGP Next Hop
In BGP, when a router advertises a route across a BGP session, i.e., between two routers running BGP, it includes the next hop attribute in the advertisement. This attribute determines the next hop IP address to use in order to reach a destination. For EBGP, the next hop attribute is updated to be the IP address of the EBGP neighbor. However, for IBGP, the routers do not update this attribute and the next hop that EBGP advertises is
propagated into the IBGP domain and by all subsequent IBGP routers.


```
AS100-CBR(config)# ip bgp 100                                                   //ASN is 100
AS100-CBR(config-router)# bgp router-id 1.1.1.1
AS100-CBR(config-router)# neighbor 192.168.12.2 remote-as 200                   //Neighbor ASN is 200
AS100-CBR(config-router)# network 192.168.1.0/24                                //The netowrks are advertised out via BGP

AS200-CBR(config)# access-list 10 permit 192.168.2.0 255.255.255.0 
AS200-CBR(config-router)# ip bgp 200
AS200-CBR(config-router)# bgp router-id 2.2.2.2
AS200-CBR(config-router)# neighbor 192.168.12.1 remote-as 100
AS200-CBR(config-router)# neighbor 192.168.23.1 remote-as 200                    //The same ASN refers to IBGP neighbors
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

## Multiprotocols and Redistributing Routing Protocols
