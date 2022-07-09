# Ethernet Frame

```
---------------------------------------------------------------------------------------
|   Preamble   |    SFD     |     Eth-Header      |         Data        | Eth-Trailer |
|    7bytes    |   1byte    |       14bytes       |    46-bytes   |    4bytes   |
| 10101010 * 7 |  10101011  |                     |                     |   FCS(CRC)  |    CRC - Cyclic Redundancy Check
--------------------------------------|------------------------------------------------
Preamble - Synchronize clock          |                                 FCS - Frame Check Sequence, detect corrupted data by running CRC algorithm on data     
SFD - Start Frame Delimeter           |
                            ------------------------------------        
                            | Dest-MAC | Src-MAC | Type/Length |        Type - value >= 1536 (IPv4_0x0800; IPv6_0x86DD)
                            |  48bit   |  48bit  |   16bits    |        Length - value =< 1500
                            ------------------------------------        
padding 0s to match minimal 46 bytes                                                                       
                                                                                                                                 
```
