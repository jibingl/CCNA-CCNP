# * 802.3 Frame and 802.11 Fram

## IEEE 802.3 (Ethernet) Frame
The size of a Ethernet frame (Eth-Header + Data + Eth-Trailer) must between 64 - 1518 bytes. If smaller, padding 0s to it; if larger, fragmented.
```
      7 bytes      1 bytes        Ehternet-Header 14 bytes          46-1500 bytes       4 bytes Eth-Trialer 
+----------------+--------+-------------------------------------+---------------------+----------+
|   Preamble     |   SFD  |  Dest-MAC  |  Src-MAC   |Type/Length|         DATA        | FCS(CRC) |
| 10101010 * 7   |10101011|   6 bytes  |   6 bytes  |   2 bytes |                     |          |  
+----------------+--------+-------------------------------------+---------------------+----------+

```
Preamble - Synchronize clock  

SFD - Start Frame Delimeter  

Type/Legnth: _Type_ is when value >= 1536 (IPv4_0x0800; IPv6_0x86DD). _Length_ is when value =< 1500.  

FCS - Frame Check Sequence, detect corrupted data by running CRC (Cyclic Redundancy Check) algorithm on data.  
> Note: _Preamble_ and _SFD_ are not calculated into the Ethernet frame length.

## IEEE 802.11 (Wireless) Frame

```
 2 bytes  2 bytes    6 bytes      6 bytes      6 bytes    2 bytes    6 bytes   2 bytes  4 bytes    various bytes     4 bytes
+-------+--------+------------+------------+------------+--------+------------+-------+---------+------------------+----------+
| Frame |Duration|   Address  |   Address  |   Address  |Sequence|   Address  | QoS   |    HT   |      DATA        | FCS(CRC) | 
|Control|  /ID   |     1      |      2     |      3     | Control|      4     |Control| Control |                  |          |
+-------+--------+------------+------------+------------+--------+------------+-------+---------+------------------+----------+
```
Frame Control: Provide info about type and subtype.

Duration/ID: Depending on message type, it can be the time dedicatedly used by the channel to transmit the frame, or identifier for the association.

Addresses: Up to 4 addresses can be present.  
 - Destination Address (DA): Final recipient
 - Source Address (SA): Original sender
 - Receiver Address (RA): Immediate recipient
 - Transmitter Address (TA): Immediate sender  

Sequence Control: TO reassemble fragments and eliminate duplicate frames.  

HT (High Throughput) Control: Added in 802.11n to enable High Throughput operations.  
