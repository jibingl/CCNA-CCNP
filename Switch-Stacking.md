# Switch Stacking
Benefits: MEC and centrolized & simplied management

## MEC (Multi-chassis EtherChannel)
- It also known as **Multi-chassis Link Aggregation Group (MLAG or MC-LAG)**.  
- **MEC** is a Layer 2 multi-path technology and a type of EtherChannel which allows a switch (S1) terminates an EtherChannel across other two physical switches (S2 and S3).  

![image](https://github.com/user-attachments/assets/86a28105-0b5f-494f-8fcf-ddaf186d37de)

## Teches of Switch Stacking

Technology \ **Params**   | VSS (Virtual Switching System) | StackWise                     | StackWise Virtual |
:-------------------------|:------------------------------:|:-----------------------------:|:-----------------:|
**# of Stacked Switches** | 2                              | Up to 8                       | 2
**Interface Type**        | Ethernet                       | Stack ports                   | Ethernet
**Interface Bandwith**    | 10G or 40G                     | Up to terabits                | 10G, 25G, 40G, or 100G
**# of Interfaces**       | 1-8                            | 2 per switch                  | 1-8
**Cables**                | Ethernet or fiber              | Stack cable                   | Ethernet or fiber
**Cable Length**          | Up to kilometers fiber         | 0.5m, 1m, 3m                  | Up to kilometers fiber
**Link Type**             | VSL (Virtual Switch Link)      | Stacking Link                 | SVL (StackWise Virtual Link)
**Link Protocols**        | LMP & RRP                      | SDP                           | LMP & SDP
**Traffic on Links**      | VSS control & data             | Control & data                | SVL control & data
**Links Topology**        |                                | Ring                          | 
**Support Switches**      | Catalyst 4500/6500/6800        | Catalyst 3750/3850, 9200/9300 | Catalyst 9400/9500/9600
**Member Roles**          | Active & Standby               | Active, Standby, & Members    | Active & Standby 
**Working Mode**          | RPR or NSF/SSO                 |                               | 

> RPR - Router-Processor Redundancy  
> NSF/SSO - Non-Stop Forwarding/Stateful Switchover  
> LMP - Link Management Protocol  
> RRP - Role Resolution Protocol  
> SDP of StackWise- Stack Discovery Protocol  
> SDP of StackWise Virtual - StackWise Discovery Protocol  

### Ring Topology
A ring topology connecting four switches, SW1, SW2, SW3 and SW4, looks like below: 
```
 .---> sp1:=SW1=:sp2 ------> sp1:=SW2=:sp2 ----.
|                                               |
 `---- sp2:=SW4=:sp1 ------> sp2:=SW3=:sp1 <---`

sp: Stack Port
```
