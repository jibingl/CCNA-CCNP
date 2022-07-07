# Basic of Serial Interfaces of Cisco routers

```
               192.168.1.1/30
    R1              cHDLC           R2
    (+)--s0/0----------------s0/0--(+)
        .1          ppp        .2
```
DCE(data communication equipment) and DTE (data terminal equipment).
_cHDCL_ is the defautl encapsulation of serial interfaces.
```
R1(config-if)#clock rate 64000                               //Configue operating speed on DCE only
R1(config-if)#ip address 192.168.1.1 255.255.255.0     
R1(config-if)#no shut
R1(config-if)#encapsulation ppp                              //Change to point-to-point encapsulation
```
