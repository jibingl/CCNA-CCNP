# Dynamic Trunking Protocol
Switch interfaces automatically negotiate/configure their _switchport_ state to be _access_ or _trunk_.  
DTP will not form a _trunk_ with a router, PC, etc. The _switchport_ will be in _access_ mode.

## Syntax
```
SW1(config-if)#switchport mode dynamic <auto | desirable>
SW1(config-if)#switchport mode nonegotiate                      //prevent DTP (negotiation) packets from being sent out the interface
SW1(config-if)#switchport trunk encapsulation <isl | dot1q>     //isl is favored by default
```
## DTP Negotiating Table
Int-states | access | auto | desirable | trunk |
---|---|---|---|---|
access | access | access | access | misconfig |
auto | access | access | trunk | trunk |
desirable | access | trunk | trunk | trunk |
trunk | misconfig | trunk | trunk | trunk |
