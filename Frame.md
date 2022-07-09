# Ethernet Frame

```
---------------------------------------------------------------------------------------
|   Preamble   |    SFD     |     Eth-Header      |         DATA        | Eth-Trailer |
|    7bytes    |   1byte    |       14bytes       |   minimum 46bytes   |    4bytes   |  FCS - Frame Check Sequence,  
| 10101010 * 7 |  10101011  |                     |     (padding 0s)    |   FCS(CRC)  |        detect corrupted data by running CRC algorithm on data  
--------------------------------------|------------------------------------------------  CRC - Cyclic Redundancy Check
Preamble - Synchronize clock          |
SFD - Start Frame Delimeter           |
                            ------------------------------------        
                            | Dest-MAC | Src-MAC | Type/Length |    Type - when value >= 1536 (IPv4_0x0800; IPv6_0x86DD)
                            |  48bit   |  48bit  |   16bits    |    Length - when value =< 1500
                            ------------------------------------                                                                              

Notes: The minimul Ethernet frame (Eth-Header + Data + Eth-Trailer) must be 64 bytes. If not, padding 0s to match the reqirement. 
```
