# Spanning Tree Protocol (Part 2)

## 🌲 BPDU (Bridge Protocol Data Unit)
BPDUs contain the root bridge ID, root path cost, and sender bridge ID, etc.   
Among those feilds, a BPDU is considered better than another due to:

(Lower)           | --->               | --->           | --->             |
------------------|--------------------|----------------|------------------|
**Superior BPDU** | The root bridge ID | Root path cost | Sender bridge ID |

> - The better a BPDU, the better the access to the best root bridge.  
> - A bridge that receives a BPDU on a port better than the one it sends out, puts this port in blocking mode unless it is its root port.

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
**UplinkFast**   | Detect a direct link failure ASAP;</br>Save convergence time of _listening_ & _learning_ stages; | Convert nd-/al-ports to _forwarding_ immediately   | Access switches with blocked _uplinks_* |
**BackboneFast** | Detect an indirect link failure ASAP;</br>Save _max_age_ timer (20S); | If receiving inferior BPDUs, age out _max_age_ after RLQ confirmation | All switches |

> On a given bridge, _uplinks_ (uplink group) consist of the root port and all blocked ports that are not self-looped, like non-designated port & alter port.
