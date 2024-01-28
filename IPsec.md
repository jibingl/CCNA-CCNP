# IPsec VPN
### Protocols
Terms | RFC Doc | Full Names |
------|---------|------------|
IKEv1 | [RFC 2409](https://datatracker.ietf.org/doc/html/rfc2409) | Internet Key Exchange |
IKEv2 | [RFC 7296](https://datatracker.ietf.org/doc/html/rfc7296) | Internet Key Exchange Version 2 |
ISAKMP| [RFC 2408](https://datatracker.ietf.org/doc/html/rfc2408) | Internet Security Association and Key Management Protocol |

IKE protocol sets up a secure and authenticated communication channel between two parties via negotiating security associations (SAs).  
 
### Modes
Type | ESP | AH |
-----|-----|----|
**Status** | Acceptable | Avoid |
**Cryptopgrahy** | Encryption, Integrity, and Authentication | Integrity and Authentication |
**Hashing Packet** | exclude new IP header | include new IP header (Whole IP packet) |

Mode | Tunnel | Transport |
-----|--------|-----------|



## Site-to-Site VPN
### Configuration Points
1. ISAKMP
2. Interesting List (Communication IPs)
3. Crypto Map (Encryption IPs)


