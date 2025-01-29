# Multiple Spanning Tree Protocol (802.1s)
More details refer to Cisco docs [here](https://www.cisco.com/c/en/us/support/docs/lan-switching/spanning-tree-protocol/24248-147.html)

Instead of creating a spanning tree instance per earch VLAN, MST groups multiple VLANs into a spanning tree instance, which dramatically reduces the number of STP instances needed.

## 

Terminology | Full Names | IEEE | Explanations |
------------|------------|------|--------------|
CST | Common Spanning Tree | 802.1d | The only one spanning tree instance is for the entire bridged network, regardless of the number of VLANs. |
MST Region |  | 802.1s | A region is a group of switches placed under a common MST instance. | 
IST | Internal Spanning Tree | 802.1s | The only one RSTP instance in MST simplely is equivalent to a CST in STP. |
MSTIs | Multiple Spanning Tree Instance(s) | 802.1s | RSTP instances only exist in a MST region. |
