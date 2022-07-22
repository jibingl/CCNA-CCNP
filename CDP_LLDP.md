# CDP & LLDP

\- CDP and LLDP can run at the same time.  
\- Only directly connectd devices can become CDP/LLDP neighbors.

Table-1 Comparation
Name | Property| Multicast Add  |Interval|Info Hold-time|reinit-Timer|
-----|---------|----------------|--------|--------------|------------|
CDP  | Cisco   | 0100.0ccc.cccc | 60s    | 180s         | N/A |
LLDP | 802.1ab | 0180.c200.000e | 30s    | 120s         | 2s  |

## CDP
CDP is enabled by default.
```
R1(config)#no cdp run            //Globally disable CDP for all interfaces
R1(config-if)#no cdp enable      //Disable CDP on a specific interface
```

`show` commands:
```
show cdp                     //Global settings info, timers, version
show cdp traffic             //CDP packets input and output
show cdp interface           //Info of the interfaces enabled CDP
show cdp neighbors           //CDP neighbors' info, including Device ID (hostname), Holdtme, 
                               Capability (router/switch/IGMP/phone), Platform (brand #), 
                               Port ID (neighbor's port), Local Interface
show cdp neighbors detail    //More info, Software version, VTP domain, Native Vlan, Duplex, IP
show cdp entry <device-id>
````

## LLDP
By default, LLDP is disabled on Cisco devices. To enable it, firstly, globally enable LLDP, then enable it on each interface.
```
R1(config)#lldp run

R1(config-if)#lldp transmit
R1(config-if)#lldp receive
```
