# Dynamic Trunking Protocol
Switch interfaces automatically negotiate/configure their _switchport_ state to be _**access**_ or _**trunk**_, also negotiate the encapsulation method.  
DTP will not form a _trunk_ with a router, PC, etc. The _switchport_ will be in _access_ mode.

## Commands & Syntax
```
SW1(config-if)#switchport mode dynamic {auto | desirable}                    //Defualt is 'auto'
SW1(config-if)#switchport mode nonegotiate                                   //prevent DTP (negotiation) packets from being sent out the interface
SW1(config-if)#switchport trunk encapsulation {negotiate | isl | dot1q}      //Configue DTP encapsulation method. By defualt, 'negotiate' is enabled and ISL is favored.
```

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
