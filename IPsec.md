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
**Encapsulation Structure** | `new-ip-header`:`ESP`:`ip-header`:`Payload`:`ESP-tail` | `ip-header`:`ESP`:`Payload`:`ESP-tail` |

> Notes: Only when `new-ip-header` and `ip-header` are the same, ***transport mode*** is avialable.

## Site-to-Site IPSec VPN
### Configuration Stages
1. IKE/ISAKMP SA (Encryption, authentication, key-exchange algorithms, respectively)
   ```
   crypto isakmp policy 10                                         //"10" is a policy number that smaller is preferred
    encryption aes                                                 //Encryption algorithm
    authentication pre-share                                       //Authentication method
    group 15                                                       //Key-exchange algorithm
   ```
2. IPsec SA (Transform Set: ESP or AH mode alongwith encryption & authentication algorithms)
   ```
   crypto ipsec transform VPN-ESP-TS esp-aes 256 esp-sha256-hmac
   ```
3. Crypto Map (Mapping isakmp with ipsec, transform set, interresting traffic, and setting peers.)
   ```
   ip access-list extend VPN permit 192.168.1.0 0.0.0.255 172.31.1.0 0.0.0.255       //The traffic needs VPN
   
   crypto map VPN-MAP isakmp-ipsec
    set transform VPN-ESP-TS
    match address list VPN
    set peer 10.10.20.1
   ```
4. Apply crypto map to interface (If applicable)
### IPSec VPN + NAT
Exclude the VPN traffic from the NAT traffic.  
As NAT is happened before IPSec, the source IP of the traffic iniated from internal subnet will be translated to the outbound public IP, which causes a mismatch with the configured _match address_ (VPN interested traffic) and no IPSec encryption will happen. 
```
ip access-list extend VPN
 deny ip <source-subnet> <destination-subnet>             //Exclude VPN traffic from NAT so that IP translations won't happen
 permit ip <source-subnet> any                            //Other traffic intend to Internet
```

## GRE over IPSec
IPSec doesn't support multicast and broadcast, so it can't be used on some protocols (like OSPF) to create VPN tunnel.
GRE creates tunnels like IPSec, but not encryp the original packets. However, it supports multicast/broadcast.
GRE-over-IPSec combines the GRE's flexibility and IPSec's security.
```
     +---------------+------------+---Encrypted---------------------------+
     |               |            |+-----------+------------+------------+|
     | New Ip Header |IPSec Header|| IP Header | GRE Header |  IP Packet ||
     |               |            |+-----------+------------+------------+|
     +---------------+------------+---------------------------------------+
```
## DMVPN (Dynamic Multiple VPN)
DMVPN is a Cisco invention for dynamically creating a full mesh of IPSec tunnels (_hub-and-spoke_ topologies) with ease, without having to manually configure every single tunnel.
