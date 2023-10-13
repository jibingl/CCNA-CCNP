# Simple Network Management Protocols
  Version | Feature
  ------- | -------
  SNMPv1  | community string password-authentication
  SNMPv2c | community string password-authentication
  SNMPv3  | encryotion and authentication

## SNMP Framework and Components
```
                            +------------------------+
                            |              NMS(SRV1) |
                            |  +------------------+  |
                            |  | SNMP Application |  |
                            |  +------------------+  |
                            |  | SNMP Manager p162|  |
                            |  +------------------+  |
                            +------------|-----------+
              (SNMP Messages)            |           (SNMP Messages)
                +------------------------+-----------------------+ 
    +-----------|------------+                      +------------|-----------+
    |  +------------------+  |                      |  +------------------+  |
    |  |  SNMP Agent p161 |  |                      |  |  SNMP Agent p161 |  |
    |  +------------------+  |                      |  +------------------+  |
    |  |        MIB       |  |                      |  |        MIB       |  |
    |  +------------------+  |                      |  +------------------+  |
    |    Managed Device(SW1) |                      |     Managed Device(R1) |
    +------------------------+                      +------------------------+

  MIB - Management Information Base, contains the variables that are identified as Object ID (OID).
```
  SNMP Object ID example:
```
    .1      .3      .6      .1      .2      .1      .1      .5
     |       |       |       |       |       |       |       |
    iso      |      dod   internet   |     mib-2     |     sysName
      identified-orgnazition        mgmt           system 
```
## Configuration
```
  R1(config)#snmp-server contact admin@mydomain.com                    //Optional info
  R1(config)#snmp-server location Winnipeg                             //Optional info
  R1(config)#snmp-server community <string1> ro                        //No 'set' message. Defualt <string1> is 'public'
  R1(config)#snmp-server community <string2> rw                        //read and write. Defualt <string2> is 'private'
  R1(config)#snmp-server host <nms-ip-address> version 2c <string1>    //Specify NMS to only read (ro)
  R1(config)#snmp-server enable traps snmap linkdown linkup            //Send traps if links are up or down
  R1(config)#snmp-server enable traps config                           //Send traps if configuration changed
```
