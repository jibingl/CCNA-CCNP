# Dynamic Trunking Protocol
- Switch interfaces automatically negotiate/configure
  - the _switchport_ states to be **access** or **trunk**; also
  - the encapsulation methods to be **ISL** or **802.1Q**.
- DTP will not form a **trunk** with a router, PC, etc. The _switchport_ will be in **access** mode.
- `switchport mode access` will disables DTP (Negotiation of Trunking) on the interface.
- To ensure DTP negotiation working, two switches' interfaces must send and receive DTP frames. It means either of interfaces with DTP (Negotiation of Trunking) off will result in misconfiguration.

## DTP Negotiation (Switchport Mode)
Switchport Modes | access |  auto  | desirable | trunk |
:---------------:|:------:|:------:|:---------:|:-----:|
**access**       | access | access | access    | X     |
**auto**         | access | access | trunk     | trunk |
**desirable**    | access | trunk  | trunk     | trunk |
**trunk**        |  X     | trunk  | trunk     | trunk |

## DTP Negotiation (Encapsulation)
Encap Modes     | Negotiate |  802.1Q  | ISL |
:--------------:|:---------:|:--------:|:---:|
**Negotiate**   | ISL       | 802.1Q   | ISL |
**802.1Q**      | 802.1Q    | 802.1Q   | X   |
**ISL**         | ISL       | X        | ISL |

## Commands & Syntax
```
SW1(config-if)#switchport mode dynamic {auto | desirable}                    //Defualt is 'auto'
SW1(config-if)#switchport mode nonegotiate                                   //prevent DTP (negotiation) packets from being sent out the interface
SW1(config-if)#switchport trunk encapsulation {negotiate | isl | dot1q}      //Configue DTP encapsulation method. By defualt, 'negotiate' is enabled and ISL is favored.
```
