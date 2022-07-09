# Ethernet Frame

```
                          ------------------------------------------------------
                          |   Eth-Header   |      Data           | Eth-Trailer |
                          |     22bytes    |                     |    4bytes   |
                          --------|------------------------------------|--------
                                  |                                    |
 -----------------------------------------------------------         ----------              Preamble - Synchronize clock
 |  Preamble  |   SFD   | Dest-MAC | Src-MAC | Type/Length |         |   FCS  |              SFD - Start Frame Delimeter
 |   56bits   |  8bits  |  48bits  |  48bits |    16bit    |         | 32bits |              Type - value >= 1536 (IPv4_0x0800; IPv6_0x86DD)
 | 10101010*7 | 10101011|          |         |             |         |        |              Length - value =< 1500
 -----------------------------------------------------------         ----------              
                                                                       FCS - Frame Check Sequence, detect corrupted data by running CRC algorithm on data
                                                                                                                                 (Cyclic Redundancy Check)
```
