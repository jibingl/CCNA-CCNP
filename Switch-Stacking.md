# Switch Stacking
Benefits: MEC and centrolized management

## MEC (Multi-chassis EtherChannel)
- It also known as **Multi-chassis Link Aggregation Group (MLAG or MC-LAG)**.  
- **MEC** is a Layer 2 multi-path technology and a type of EtherChannel which allows a switch (S1) terminates an EtherChannel across other two physical switches (S2 and S3).  

![image](https://github.com/user-attachments/assets/86a28105-0b5f-494f-8fcf-ddaf186d37de)

## Teches of Switch Stacking

### VSS (Virtual Switching System)

![image](https://github.com/user-attachments/assets/044774ea-0ed6-4f02-8699-5fb8c4379bae)

### StackWise



### StacWise Virtual



Technology/***Params***  | VSS (Virtual Switching System) | StackWise                     | StackWise Virtual |
:------------------------|:------------------------------:|:-----------------------------:|:-----------------:|
***Stacked Switches #*** | 2                              | Up to 8                       | 2
***Interface Type***     | Ethernet                       | Stack ports                   |
***Interface Bandwith*** | 10G - 40G                      | Up to terabits                |
***Interface #***        | 1-8                            | 2 per switch                  |
***Link Type***          | VSL (Virtual Switch Link)      |                               | SVL (StackWise Virtual Link)
***Link Protocols***     | LMP, RRP                       | SDP                           |
***Link Cables***        | Ethernet or Fiber              | Stack cables                  |
***Cable Length***       | Up to kilometers by fiber      | 0.5m, 1m, 3m                  |
***Traffic on Link***    | VSS control & data             | Control & data                |
***Link Topology***      |                                | Ring connection               |
***Support Switches***   | Catalyst 4500/6500/6800        | Catalyst 3750/3850, 9200/9300 | Catalyst 9400/9500/9600
***Work Modes***         | RPR or NSF/SSO                 |                               |
***Members Roles***      | Active & Standby               | Active, Standby, & Members    |

RPR - Router-Processor Redundancy
NSF/SSO - Non-Stop Forwarding/Stateful Switchover
LMP - Link Management Protocol
RRP - Role Resolution Protocol
Ring topology - S1 -> S2 -> S3 -> S4 -> S1
SDP - Stack Discovery Protocol
