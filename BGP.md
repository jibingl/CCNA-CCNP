# Border Gateway Protocol
BGP is an exterior gateway protocol (EGP) designed to exchange routing and reachability information among ASes on the Internet. 
- iBGP (Internal BGP) - Within an AS
- eBGP (External BGP) - AS to AS

Each AS is identified by an ASN that is either public or private. A public ASN is globally unique and can be advertised across the Internet; however, a private ASN is not globally unique and should not be advertised to external networks. Private ASNs range from 64512 to 65534, and from 4,200,000,000 to 4,294,967,294. All other ASNs are public and available for use on the Internet except for a few reserved numbers.  

A public ASN is required only when an AS is originating routes that are visible on the Internet. However, a private ASN should be used when an AS is only exchanging routes via BGP with a single Internet Service Provider (ISP).  
> E.g. The AS of the customer is assigned a private ASN (64512) since the customer is connected to one ISP via BGP. The ISP has a public ASN (100) since it originates the routes that are visible on the Internet.   

## Attributes
There are many attributes used to decide BGP path selection of routers. Below are some.  
Attributes/Params  | About Path Selection | Preferred | Routing Direction |
-------------------|----------------------|-----------|-------------------|
(1) Weight         | A locally signaficant, Cisco-specific param that a router can set when reveiving updates | Higher | Commanly influence outbound |
(2) LOCAL_PREF     | A param communicated throughout a single AS. | Higher | Commanly influence outbound |
(3) Originate      | "If I own connect to the network directly?" | Locally |  |
(4) AS Path Length | The number of AS in the AS_PATH attribute | Lower | Commonly influence inbound |
(5) Origin Type    | How the route was injected into BGP: i(`network` cmd), e(EGP), or ?(redistributed). | i > e > ? |
(6) MED            | Set and Advertised to influence routers selection within another AS. | Lower | Influence another AS |
(7) Paths          | | eBGP > iBGP | |
(8) Router ID      | | Lowest | |

![image](https://github.com/jibingl/CCNA-CCNP/assets/84643474/587c16bd-9b10-4d72-b6b0-a78c41697a3d)

### AS_PATH
The AS_PATH attribute is a list of all ASes that a specific route passes through to reach a specified network. When a router is advertising a BGP route, the AS_PATH attribute is first created empty. Each time the route is advertised from one AS to another, the AS_PATH attribute is modified to prepend the ASN of the router that advertised the route.

Routers use the AS_PATH attribute to detect and prevent loops. For example, a router drops any route in which its own ASN is part of the AS_PATH attribute.

Private ASNs are not globally unique, hence, they cannot be leaked to the Internet. To achieve this goal, routers must strip the private ASNs from the AS_PATH attribute list before the routes are advertised to the Internet.

### LOCAL_PREF
A router sending iBGP update packets will include the LOCAL_PREF attribute. This attribute indicates the degree of preference for one BGP route over the others when an AS has multiple routes to another AS. The LOCAL_PREF attribute is always advertised to iBGP neighbors and never advertised to eBGP peers, hence, it is exchanged only among routers within the same AS. The BGP route with the highest LOCAL_PREF value is preferred.
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

### MED
The MED (Multi Exit Discriminator) attribute indicates to external neighbors the preferred path into an AS if there are multiple entry points into the same AS. The BGP route with the lowest MED value is preferred.  
The MED is sent to eBGP peers; those peer routers propagate the MED attribute within their AS, but do not pass it on to the next AS.
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

## AS_PATH Prepending
The BGP AS_PATH attribute is a well-known mandatory attribute that is present for all prefixes exchanged between BGP neighbors. When a BGP router sends a BGP advertisement to an EBGP peer, it adds its own AS number to the front of the AS_PATH.   
AS_PATH prepending is the process of adding one or more AS Number (ASN) to the front of the AS_PATH when advertising a BGP prefix.
```
(config)# route-map prepend_out permit 10
(config-route-map)# set as-path prepend 100 100
(config-route-map)# exit
(config)# router bgp 100
(config-router)# neighbor 192.168.45.2 route-map prepend_out out
```

## Nerghbors Forming
Kevin Wallace Training, LLC  
![image](https://github.com/jibingl/CCNA-CCNP/assets/84643474/0b551eaa-1b90-48cb-be75-27f096dd4499)


## Commands
```
AS100-CBR(config)# router bgp 100                                                   //ASN is 100
AS100-CBR(config-router)# bgp router-id 1.1.1.1
AS100-CBR(config-router)# neighbor 192.168.12.2 remote-as 200                   //Explicitly config a neighbor with ASN 200
AS100-CBR(config-router)# network 192.168.1.0/24                                //The netowrk we want to advertise out via BGP

AS200-CBR(config)# access-list 10 permit 192.168.2.0 255.255.255.0 
AS200-CBR(config-router)# router bgp 200
AS200-CBR(config-router)# bgp router-id 2.2.2.2
AS200-CBR(config-router)# neighbor 192.168.12.1 remote-as 100
AS200-CBR(config-router)# neighbor 192.168.23.1 remote-as 200                    //The same ASN refers to an iBGP neighbor
AS200-CBR(config-router)# network 192.168.2.0/24
AS200-CBR(config-router)# neighbor 192.168.12.1 password cisco                   //BGP peers authentication
AS200-CBR(config-router)# neighbor 192.168.12.1 distribute-list 10 out            //Filter routing advertisement
AS200-CBR(config-router)# neighbor 192.168.12.1 default-orginate                 //Advertise default route out via BGP

AS300-ISP-BR1(config)#
AS300-ISP-BR1(config-router)#

AS300-ISP-BR2(config)#
AS300-ISP-BR2(config-router)# neighbor 192.168.23.2 remove-private-AS            //Remove private ASN information from AS_PATH attributes that received from a neighbor
AS300-ISP-BR2(config)# bgp as-path access-list 1 deny ^100$                      //Matches only AS_PATHs that are sourced from AS 100
AS300-ISP-BR2(config)# bgp as-path access-list 1 permit .*                       //Matches any AS_PATH that has not been denied by the previous ACL statement
AS300-ISP-BR2(config-router)# neighbor 192.168.23.2 filter-list 1 out
```

## Multiprotocols and Redistributing Routing Protocols


## Full Mesh IBGP topology
A topology is called full mesh (fully meshed topology) when there is an IBGP peering relationship between any two routers in the AS. All BGP routers within a single AS must be fully meshed so that any external routing information must be re-distributed to all other routers within that AS.

## BGP Route Reflection
A BGP route reflector is an IBGP router (R1) that repeats routes learned from IBGP peers (R2 and R3) to some of its other IBGP peers, its clients (R4, R5, and R6).
```
neighbor 192.168.23.1 route-reflector-client
neighbor 192.168.34.2 route-reflector-client
```
