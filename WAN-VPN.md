# WAN and/vs VPN

WAN connection services include leased line, Ethernet, DSL, CATV, Fiber, and WLAN.

  Glossary         | Explanation
  -----------------|------------
  Hub-and-Spoke    | A WAN topology like center-branches
  DSL              | Digital Subscriber Line
  Modem            | Modulator-demodulator
  PSTN             | Public Switched Telephone Network
  CATV             | Cable Television
  CE router        | Customer Edge router
  PE router        | Provider Edge router
  P router         | Provider router
  MPLS             | Multi-Protocol Label Switching
  Single-homed     | Customer's 1 connection to 1 ISP
  Dual-homed       | Customer's 2 connections to 1 ISP
  Multihomed       | Customer's 2 connections to 2 ISPs
  Dual Multihomed  | Customer's 2 connections to each 2 ISPs
  DMVPN            | Dynamic Multipoint VPN
  
## MPLS VPN
Label switching - forwarding decision based on labels, not destination IP.
Create VPNs over MPLS infrastructure.
```  
                     .---------ISP-MPLS-net--.
                    /                         \
     CE-R1         |PE-R2       P-R1      PE-R3|           CE-R2
      (+)---------(+)-----------(+)-----------(+)----------(+)
                   |===========================|
                    \   L3-MPLS-VPN-tunnel    /
                     `-----------------------`

                     .-------ISP-(like a SW)-.
                    /                         \
     CE-R1         |PE-R2       P-R1      PE-R3|           CE-R2
      (+)---------(+)-----------(+)-----------(+)----------(+)
         ==========|===========================|===========
                    \   L2-MPLS-VPN-tunnel    /
                     `-----------------------`
```

## VPN Concept
```
     +---------------+------------+---Encrypted------------------------+
     |               |            |+-----------+-----------+----------+|
     | New L3-header | VPN-header || L3-header | L4-header |   data   ||
     |               |            |+-----------+-----------+----------+|
     +---------------+------------+------------------------------------+
```
## IPSec and TLS VPNs
IPSec commonly is for site-to-site VPN, while TLS VPN conmmanly is for remote access.

## GRE over IPSec
IPSec doesn't support multicast and broadcast, so it can't be used on some protocols (like OSPF) to create VPN tunnel.
GRE creates tunnels like IPSec, but not encryp the original packets. However, it supports multicast/broadcast.
GRE-over-IPSec combines the GRE's flexibility and IPSec's security.
```
     +---------------+------------+---Encrypted---------------------------+
     |               |            |+-----------+------------+------------+|
     | New Ip Header |IPSec Header|| IP Header | GRE Header |  IP Packet ||
     |               |            |+-----------+------------+------------+|
     +---------------+------------+---------------------------------------+
```
## DMVPN (Dynamic Multiple VPN)
DMVPN allows is a Cisco invention for dynamically creating a full mesh of IPSec tunnels (_hub-and-spoke_ topologies) with ease, without having to manually configure every single tunnel.
