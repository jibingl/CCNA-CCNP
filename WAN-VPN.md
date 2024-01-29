# WAN and/vs VPN
## WAN
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
