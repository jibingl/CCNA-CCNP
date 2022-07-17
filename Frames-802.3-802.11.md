# * 802.3 Frame and 802.11 Fram

## IEEE 802.3 (Ethernet) Frame
The size of a Ethernet frame (Eth-Header + Data + Eth-Trailer) must between 64 - 1518 bytes. If smaller, padding 0s to it; if larger, fragmented.
```
      7 bytes      1 bytes        Ehternet-Header 14 bytes          46-1500 bytes       4 bytes Eth-Trialer 
+----------------+--------+-------------------------------------+---------------------+----------+
|   Preamble     |   SFD  |  Dest-MAC  |  Src-MAC   |Type/Length|         DATA        | FCS(CRC) |  FCS - Frame Check Sequence,
| 10101010 * 7   |10101011|   6 bytes  |   6 bytes  |   2 bytes |                     |          |        detect corrupted data by running CRC algorithm on data  
+----------------+--------+-------------------------------------+---------------------+----------+  CRC - Cyclic Redundancy Check
Preamble - Synchronize clock                            Type - when value >= 1536 (IPv4_0x0800; IPv6_0x86DD)
SFD - Start Frame Delimeter                             Length - when value =< 1500
```
  > Note: _Preamble_ and _SFD_ are not calculated into the Ethernet frame length.

## IEEE 802.11 (Wireless) Frame

```
 2 bytes  2 bytes    6 bytes      6 bytes      6 bytes    2 bytes    6 bytes   2 bytes  4 bytes    46-1500 bytes     4 bytes
+-------+--------+------------+------------+------------+--------+------------+-------+---------+------------------+----------+
| Frame |Duration|   Address  |   Address  |   Address  |Sequence|   Address  | QoS   |    HT   |      DATA        | FCS(CRC) | 
|Control|   ID   |     1      |      2     |      3     | Control|      4     |Control| Control |                  |          |
+-------+--------+------------+------------+------------+--------+------------+-------+---------+------------------+----------+
```

