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
**Encapsulation Structure** | `new-ip-header`:`ESP`:`init-ip-header`:`Payload`:`ESP-tail` | `ip-header`:`ESP`:`Payload`:`ESP-tail` |

> Notes: Only when `new-ip-header` and `init-ip-header` are the same, ***transport mode*** is avialable.

## Site-to-Site VPN
### Configuration Sections
1. IKE/ISAKMP (Encryption, authentication, key-exchange algorithms, respectively)
   ```
   crypto isakmp policy 10
    encryption aes
    authentication pre-share
    group 15
   ```
2. IPsec mode (Transform Set: ESP or AH mode alongwith encryption & authentication algorithms)
   ```
   crypto ipsec transform VPN-ESP-TS esp-aes 256 esp-sha256-hmac
   ```
3. Crypto Map (Mapping isakmp with ipsec, transform set, interresting traffic, and setting peers.)
   ```
   access-list VPN permit 192.168.1.0 0.0.0.255 172.31.1.0 0.0.0.255
   
   crypto map VPN-MAP isakmp-ipsec
    set transform VPN-ESP-TS
    match address list VPN
    set peer 10.10.20.1
   ```
6. Apply crypto map to interface (If applicable)

