# Software-Defined Networking
An networking approach that centralizes the _control plane_ into an application called a _controller_.

SBI - South-bound Interface - controller-to-devices
NBI - North-bound Interface - human-cotrollers

```
   SDN Architecture
                           +------------+
                           |    Apps    |
                           +------------+
                               |     |                   Application Layer
   -------------------------( REST API )-------------------------------------
                               | NBI |
                        +---+----------+---+
                        |     Controller   |
                        | .--------------. |
                        | |Controll Plane| |
                        | `--------------` |
                        +------------------+
                           |     SBI    |                Controll Layer
   ------------------------|--( API )---|------------------------------------
                          /              \               Infrastructure Layer
                 +---(+)---+            +---(+)---+
                 |   R1    |            |   R2    |
                 | .-----. |            | .-----. |
                 | |Data | |            | |Data | |
   ---(packet)---| |Plane| |--(packet)--| |Plane| |---(packet)---
                 | `-----` |            | `-----` |
                 +---------+            +---------+
```
 > Notes: NBI - Northbound Interface); SBI - Southbound Interface 


