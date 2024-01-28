# IPsec VPN

### Protocols
Terms | RFC Doc | Full Names |
------|---------|------------|
**IKEv1** | [RFC 2409](https://datatracker.ietf.org/doc/html/rfc2409) | Internet Key Exchange |
**IKEv2** | [RFC 7296](https://datatracker.ietf.org/doc/html/rfc7296) | Internet Key Exchange Version 2 |
**ISAKMP**| [RFC 2408](https://datatracker.ietf.org/doc/html/rfc2408) | Internet Security Association and Key Management Protocol |

IKE protocol formerly is referred to as ISAKMP/Oakley. It negotiates IPsec parameters (aka. security associations (SAs)) between devices.  
 
### Operation Modes
Type               | ESP | AH |
-------------------|-----|----|
**Status**         | Acceptable | Avoid |
**Cryptopgrahy**   | Encryption, Integrity, and Authentication | Integrity and Authentication |
**Hashing Packet** | exclude new IP header | include new IP header (Whole IP packet) |

Mode | Tunnel | Transport |
-----|--------|-----------|
**Encrypting** | (Initial) IP Header & Payload | Payload |
**Encapsulating** | `new-ip-header`:`ESP`:`init-ip-header`:`Payload`:`ESP-tail` | `ip-header`:`ESP`:`Payload`:`ESP-tail` |

> Notes: Only when `new-ip-header` and `init-ip-header` are the same, ***transport mode*** is avialable.

## Site-to-Site VPN
### Configuration Points
1. ISAKMP
2. Interesting List (Communication IPs)
3. Crypto Map (Encryption IPs)

an ESP encryption transform
an ESP authentication transform
an AH transform
