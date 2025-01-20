# Spanning Tree Protocol (Part 2)

## 🌲 PortFast, BPDU-Guard, BPDU-Filter, & RootGuard
 Features      | Purposes                                                         | Implementations                                                      | Practically Enable on |
:-------------:|------------------------------------------------------------------|----------------------------------------------------------------------|-----------------------|
**PortFast**   | Save convergence time of _listening_ & _learning_ stages         | Start _forwarding_ immediately; if receiving BPDUs, disable PortFast.| Access ports |
**BPDU-Guard** | Against switches being connected to ports intended to end hosts  | Don't accept BPDUs, otherwise put the port into errdisable.          | Access ports along with PortFast |
**BPDU-Filter**| Avoid errdisable while achieving BPDU Guard purpose              | Don't receive & send BPDUs, and ignore recieved BPDU.                | Access ports along with PortFast |
**RootGuard**  | Prevent unwanted switches from becoming root bridge              | If receiving superior BPDUs, put the port into _root inconsistent_.  | Designated ports where root bridge must not appear |

## 🌲 UplinkFast & BackboneFast
 Features        | Purposes                                                 | Implementtations                                   | Practically Enable on |
:---------------:|----------------------------------------------------------|----------------------------------------------------|-----------------------|
**UplinkFast**   | Save convergence time of _listening_ & _learning_ stages | Convert nd-/al-ports to _forwarding_ immediately   | Access switches with blocked _uplinks_* |
**BackboneFast** | Save _max_age_ timer (20S)                               | If receiving inferior BPDUs, age out the _max_age_ | All switches |
> On a given bridge, _uplinks_ (uplink group) consist of the root port and all blocked ports that are not self-looped, like nondesignated-port & alter-port.

## 🌲 BPDU
BPDUs and How to Compare Them
Bridge Protocol Data Units (BPDUs) can be strictly classified by the fields they carry. Among these fields are the root bridge ID, path cost to the root, and sender bridge ID. A BPDU is considered better than another BDPU for these reasons:

When one BPDU carries a better root bridge ID than another. The lower the value, the better.

When the root bridge ID values are equal, then the BPDU with the lowest path cost to the root is better.

When the root bridge ID values are equal and the costs to the root are the same, then the BPDU with the better sender bridge ID is better. The lower the value, the better.

There are other variables that then can act as a tie-breaker. However, the better a BPDU, the better the access to the best root bridge.

A bridge that receives a BPDU on a port better than the one it sends out, puts this port in blocking mode unless it is its root port. This means that on the segment connected to this port, there is another bridge that is a designated bridge. A bridge stores the value of the BPDU on a port sent by the current designated bridge.
