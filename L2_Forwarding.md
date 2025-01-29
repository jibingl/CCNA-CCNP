# Network Traffic Forwarding
 Layers | Match Rules         | What's done at the layer? |
--------|---------------------|---------------------------|
Layer 2 | Exact match         | Forwarding                |

#### Source MAC Check

On the local switch, MAC-address table is enumerated to look for the source MAC address.
```
                                                                      Yes, reset time stamp; send to dest's port
                                                                    /
Source MAC ------ Check MAC-Address Table ------ Has the source MAC?
                                                                    \
                                                                      No, add an record; flood out

```
