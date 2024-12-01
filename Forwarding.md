# Network Traffic Forwarding

## Layer 3 Forwarding

### Bitwise Operation

On the source host, the ***Bitwise operation*** determines to where a network package should be forward.
```
                  ..-------.
     Source IP ----\\       \              .-------.                          Yes, send to destination
                    ||  XOR  . ------------|        \                        /
Destination IP ----//       /              |   AND   | --------- Result zero?    
                  ``-------`       .-------|        /                        \
                                   |       `-------`                           No, sned to default gateway
                        Source IP mask
```
Examples  | Same subnet                          | Different subnet
----------|--------------------------------------|--------------------------------------
---->     |From `192.168.1.100/24` to `192.168.1.200/24` |From `192.168.1.100/24` to `192.168.14.100/24`
Sour IP   | 11000000.10101000.00000001.01100100  | 11000000.10101000.00000001.01100100
Dest IP   | 11000000.10101000.00000001.11001000  | 11000000.10101000.00001110.01100100
***XOR*** | 00000000.00000000.00000000.10101100  | 00000000.00000000.00001111.00000000
Sour Mask | 11111111.11111111.11111111.00000000  | 11111111.11111111.11111111.00000000
***AND*** | 00000000.00000000.00000000.00000000  | 00000000.00000000.00001111.00000000

### Routing Table
Longest mactch of prefix

### Directed Broadcast
Enable the router connected to the destination subnets to not drop but forward a broadcast package into the destination subnet.

## Layer 2 Forwarding

### Source MAC Check

On the local switch, MAC-address table is enumerated to look for the source MAC address.
```
                                                                      Yes, reset time stamp; send to dest's port
                                                                    /
Source MAC ------ Check MAC-Address Table ------ Has the source MAC?
                                                                    \
                                                                      No, add an record; flood out

```
