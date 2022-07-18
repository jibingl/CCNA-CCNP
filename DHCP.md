# Dynamic Host Configuration Protocl

## IPv4 DHCP
RFC 2131
```
                Server          Client          Server
            (not selected)                    (selected)
                UDP 67          UDP 68          UDP 67    
                  v               v               v
                  |               |               |
                  |     Begins initialization     |
                  |               |               |
                  | _____________/|\____________  |
                  |/DHCPDISCOVER  | DHCPDISCOVER \|              Broadcast
                  |               |               |
              Determines          |          Determines
             configuration        |         configuration
                  |               |               |
                  |\              |  ____________/|
                  | \________     | /DHCPOFFER    |              Broadcast/Unicast
                  | DHCPOFFER\    |/              |
                  |           \   |               |
                  |       Collects replies        |
                  |             \ |               |
                  |     Selects configuration     |
                  |               |               |
                  | _____________/|\____________  |
                  |/ DHCPREQUEST  |  DHCPREQUEST\ |              Broadcast
                  |               |               |
                  |               |     Commits configuration
                  |               |               |
                  |               | _____________/|
                  |               |/ DHCPACK      |              Broadcast/Unicast
                  |               |               |
                  |    Initialization complete    |
                  |               |               |
                  .               .               .
                  .               .               .
                  |               |               |
                  |      Graceful shutdown        |
                  |               |               |
                  |               |\ ____________ |
                  |               | DHCPRELEASE  \|
                  |               |               |
                  |               |        Discards lease
                  |               |               |
                  v               v               v
     Figure 3: Timeline diagram of messages exchanged between DHCP
       client and servers when allocating a new network address
```
## IPv6 DHCP
Before DHCP process starts, clients and servers (routers) exchange info by using NDP (part of ICMPv6) to determine how to get IPv6 address.
```
                Server          Client          Server
            (not selected)                    (selected)
               UDP 547          UDP 546         UDP 547
                  v               v               v
                  |               |               |
                  |     Begins initialization     |
                  |               |               |
     start of     | _____________/|\_____________ |
     4-message    |/    RS        |        RS    \|              Multicast ff02:2
     exchange     |               |               |
              Determines          |          Determines
             configuration        |         configuration
                  |               |               |
                  |\              |  ____________/|
                  | \________     | /     RA      |              Multicast ff02:1
                  |      RA  \    |/              |
                  |           \   |               |              A=1, O=0, M=0 SLAAC
                  |      Collects Advertises      |              A=1, O=1, M=0 
                  |             \ |               |              A=0, O=0, M=1 Stateful (DHCP)
                  |     Selects configuration     |
                  |               |               |
                  v               v               v
```
RFC 8415
```
                Server          Client          Server
            (not selected)                    (selected)
               UDP 547          UDP 546         UDP 547
                  v               v               v
                  |               |               |
                  |     Begins initialization     |
                  |               |               |
     start of     | _____________/|\_____________ |
     4-message    |/ Solicit      | Solicit      \|              Multicast ff02::1:2
     exchange     |               |               |
              Determines          |          Determines
             configuration        |         configuration
                  |               |               |
                  |\              |  ____________/|
                  | \________     | /Advertise    |              Unicast
                  | Advertise\    |/              |
                  |           \   |               |
                  |      Collects Advertises      |
                  |             \ |               |
                  |     Selects configuration     |
                  |               |               |
                  | _____________/|\_____________ |
                  |/ Request      |  Request     \|              Unicast
                  |               |               |
                  |               |     Commits configuration
                  |               |               |
     end of       |               | _____________/|
     4-message    |               |/ Reply        |              Unicast
     exchange     |               |               |
                  |    Initialization complete    |
                  |               |               |
                  .               .               .
                  .               .               .
                  |   T1 (renewal) timer expires  |
                  |               |               |
     2-message    | _____________/|\_____________ |
     exchange     |/ Renew        |  Renew       \|              Unicast
                  |               |               |
                  |               | Commits extended lease(s)
                  |               |               |
                  |               | _____________/|
                  |               |/ Reply        |              Unicast
                  .               .               .
                  .               .               .
                  |               |               |
                  |      Graceful shutdown        |
                  |               |               |
     2-message    | _____________/|\_____________ |
     exchange     |/ Release      |  Release     \|
                  |               |               |
                  |               |         Discards lease(s)
                  |               |               |
                  |               | _____________/|
                  |               |/ Reply        |
                  |               |               |
                  v               v               v

   Figure 9: Timeline Diagram of the Messages Exchanged between a Client
      and Two Servers for the Typical Lifecycle of One or More Leases
```
