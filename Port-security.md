# Switch Port Security
Only work for access port on layer 2 through managing MAC addresses that connect to a switch port.

## Deafult Settings and Behavior
Max MAC| Violation mode| Port-stat befor violation| Port-stat after violation| Port-recovery|
-------|---------------|--------------------------|--------------------------|--------------|
1 | shutdown | secure-up| error-disablea | manully|

## Violation Modes
Violation mode| Port disabled| Data drop| Syslog| Display error| Increase Violation Counter|
--------------|--------------|----------|-------|--------------|---------------------------|
**shutdown**| Yes| Yes| No| No| Yes|
**restrict**| Yes| Yes| Yes| No| Yes|
**protect**| No| Yes| No| No| No|

## Recommand Configurations
Max MAC| Violation mode| Port recovery| MAC learning| Combining configs|
-------|---------------|--------------|-------------|------------------|
2 | no **protect** | automatically (`errdiable recovery`)| `sticky`| `dhcp snooping`, IDA|

```
SW1(config)#interface f0/1
SW1(config-if)#switchport mode access                          //Port-security only be configured on access ports 
SW1(config-if)#switchport port-security maximum 2
SW1(config-if)#switchport port-security mac-address sticky     //Rmember to copy running-configure to start-configure after MAC address learned and sticked
SW1(config-if)#switchport port-security                        //Remeber enable port-security after above configuring
SW1(config-if)#exit

SW1(config)#errdisable recovery interval 300                   //300 seconds
SW1(config)#ip dhcp snooping
SW1(config)#ip dhcp snooping vlan <vlan-ids>
SW1(config)#interface g0/0
SW1(config-if)#ip dhcp snooping trust                          //Default ports are untrusted after enabling dhcp-snooping
SW1(config-if)#exit

SW1(config)#ip arp inspection vlan <vlan-ids>
SW1(config)#interface f0/1
SW1(config-if)#ip arp inspection trust                         //Optional, if don't want perform inspection on a port
```
