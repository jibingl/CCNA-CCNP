# Neighbor Discovery Protocol

Abbr|Names|ICMPv6 Tpyes|Description|Provide Functions|
----|---|------------|-----------|-----|
RS | Router Solicitation | Type 133 | Locate routers on a local link; Sent to multicast address ff02::2 | |
RA | Router Advertisement | Type 134 | Response to RS or sent periodically to announce presences and other info; Sent to multicast address ff02::1| |
NS | Neighbor Solicitation  | Type 135 | Discover a neighbor's MAC address; Verify if a neighbor is still reachable | 'ARP', DAD|
NA | Neighbor Advertisement | Type 136 | Response to NS, or unsolicited to provide info |  'ARP', DAD|
|  | Redirect | Type 137 | A better next-hop route for a destination | |

**DAD (Duplicate Address Detection)**
 - A host send an NS to its own IPv6 address. 
 - If no response get, it knows the address is unique.
 - If get a reply, it means another host on the network is already using the address.
