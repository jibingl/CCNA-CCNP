# Address Resolution Protocol

![image](https://github.com/jibingl/CCNA-CCNP/assets/84643474/c833f7f1-d4b0-4c69-b957-6918bcccf93e)
- There is no IP header.
- **Type** of **0x0806** is ARP.
- ARP messages are variable in length. IPv4/Ethernet ARP message is 28 bytes.

![image](https://github.com/jibingl/CCNA-CCNP/assets/84643474/a4b6ebb4-296d-4bda-bb3f-d344e30a62cd)
- **HTYPE** - L2 protocol type. Value 1 is Ethernet.
- **PTYPE** - L3 protocol type. Value 0x0800 (0d2048) is IPv4.
- **HLEN** - Length (byte) of L2 address. Value 6 is MAC address.
- **PLEN** - Length (byte) of L3 address. Value 4 is IPv4.
- **OPER** - Type of ARP message. 1 is for request, 2 is for reply.
- **SHA** - Sender's MAC address.
- **SPA** - Sender's L2 address.
- **THA** - Intended receiver's L2 address.
- **TPA** - Intended reveiver's L3 address.

## Proxy-ARP
#### Case #1 - PC1 can ping PC3/PC4 even whitout default gateway configured
![image](https://github.com/jibingl/CCNA-CCNP/assets/84643474/d878b399-e4b1-4ecb-84d1-204766562aa5)

#### Case #2 - R1 G0/0 is on behalf of 192.168.34.0/24
![image](https://github.com/jibingl/CCNA-CCNP/assets/84643474/01389521-5d5d-4e46-9025-13715df43392)
