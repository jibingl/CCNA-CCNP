# Wireless Network

## Fundamentals
### _WiFi Table_
WiFi-Alliance | IEEE | Max-bps | Freqency |
---------|------|-----------|----------|
WiFi | 802.11 | 2M | 2.4G |
WiFi 1 | 802.11b | 11M | 2.4G |
WiFi 2 | 802.11a | 54M | 5G |
WiFi 3 | 802.11g | 54M | 2.4G |
WiFi 4 | 802.11n | 600M | 2.4G/5G |
WiFi 5 | 802.11ac | 6.9G | 5G |
WiFi 6 | 802.11ax | 9.6G | 2.4G/5G |
WiFi 6E | 802.11ax | 4\*802.11ac | 2.4G/5G/6G |
WiFi 7 | 802.11be | 40G | 2.4G/5G/6G |

### _Wireless signal coverage impacted by:_
 - absorption 吸收 Wireless signal partially is converted into heat during passing through a material.
 - reflection 反射 Wireless signal bounces off of a material.
 - refraction 折射 A signal wave is bent when entering a medium where the signal travels at a different speed.
 - diffraction 衍射 A wave encounters an obstacle and travels around it.
 - scattering 散射 A material causes a signal to scatter in all directions.
 
### _2.4G and 5G_
Name | Hz Range | Total Channels | Channel range | 
-----|----------|----------------|---------------|
2.4G | 2.4G - 2.4835G | 14 (1-11 usable in NA/1-13 in other) | 22MHz |
5G   | 5.15G - 5.825G |  | MHz |

### _Service Sets_
**IBSS** (Independent Basic Service Set) - Two or more wireless devices connect directly without using an AP.  
Aka **ad hoc**. An example is Apple _AirDrop_.  

**BSS** (Basic Service Set) - A kind of Infrastracture Service Set in which clients connect to each other via an AP.  
BSSID (Basic Service Set ID) is used to uniquely identify the AP.  
BSA (Basic Service Area) is the physical area around an AP where its signal can cover.  
When multiple BSSs separately provide service, each BSS is mapped to a separate VLAN on the wired network, and each AP should connect to switch via _trunk_.

**ESS** (Extended Service Set) - Multiple BSSs connect together. Each BSS uses the same SSID, but has own unique BSSID. Each BSS must uses a fifferent channnel to avoid interference.  
Roaming, wireless devices can seamless travel between BSSs.

**MBSS** (Mesh Basic Service Set) - used in situations where it's difficult to run an Ethernet connection to every AP.  
MAP (Mesh Access Point), those APs deployed in a MBSS, except one called RAP.   
RAP (Root Access Point), the AP is directly connected to the wired network. A MBSS at least has one RAP.   
Two radios are used by MAPs: one to provide a BSS, the other to form a 'backhaul network' to bridge traffic from AP to AP.  

### _Additional AP Operational Modes_
1. Repeater
2. Workgroup bridge (WGB)
3. Outdoor bridge

```
                       .-PC--.      .-----.
                     /   ch-1  \  /  ch-1   \
   SW              /            /\            \ 
   [=]--ethernet--|---<<AP1>>  |  | <<AP2>>>>>>PC
                   \            \/  Repeater  /
                     \         /  \         /
                       `-PC--`      `-----`

                       .-PC--.         
                     /   ch-1  \      
   SW              /             \ WGB   
   [=]--ethernet--|---<<AP1>>   <<AP2>>---ethernet--PC
                   \             /   
                     \         /       
                       `-PC--`          

                                                                        PC
        SW            outdoor-bridge          outdoor-bridge            |
   PC--[=]--ethernet----<<AP1>>>>>>>>><<<<<<<<<<<<<<AP2>>----ethernet--[=]---PC
        |                                                               |
        PC                                                              PC
```

## Wireless Architectures
### APs Deployment Modes
AP Deployment | To-WLC | APs Config | APs-To-Wired-Net | WLC-to-wired-Net | Notes | 
--------------|--------|------------|------------------|------------------|-------|
Autonomous AP | N/A | Individually | Trunk | N/A |Each AP needs Mngt IP & RF parameters |
Lightweight AP | CAPWAP | Centrally | Access | Trunk | WLC & APs authenticate each other by x.509 |
Cloud-based AP | Cloud/Meraki | Centally | | | _Autonomous_ APs are centrally managed in the cloud |

#### _Lightweight AP Deployment_
**Lightweight** APs deployment is also known as _split-MAC archtecture_.

**CAPWAP** (Control And Provisioning of Wireless Access Points): a protocol for communication between lightweight APs and WLC.
 - Control Tunnel: encrypted. UDP port 5246.
 - Data Tunnedl: not encrypted but can be configured to. UDP port 5247.

Lightweight APs operational modes:
Modes | Functions | 
------|-----------|
Local | Offer BSS or BSSs (default mode) |
FlexConnect | Local + _autonomous_ deploment (when WLC is lost) |
Sniffer | Dedicated to capture 802.11 frames and send to a sniffer app (WireShark) |
Monitor | Decicated to receive 802.11 frames and detect rogue devices |
Rogue Detector | Dedicated to work with WLC to listen to wired traffic only and detect rogue devices |
SE-Connect | Dedicated to RF spectrum analysis on all channels |
Bridge/Mesh | Like autonomous AP's _outdoor Bridge_ |
Flex plus Bridge | Add _FlexConnect_ to _Bridge/Mesh_ |

### WLC Deployment Modes
Deploment modes | Description | Support |
----------------|-------------|---------|
Unified WLC| Hareware WLC | 6000 APs |
Cloud-based WLC | VM WLC | 3000 APs |
Embedded WLC | WLC is integrated wiht a switch | 200 APs |
Mobility Express WLC | WLC is intergated within an AP | 100 APs |

## WiFi Encryptions
            | WEP | WPA | WPA2 |WPA3 |
------------|-----|-----|------|-----|
Release Year| 1999| 2003| 2004| 2018 |
Authentication| WEP-open & WEP-sharedkey| PSK & 802.1x| PSK & 802.1x| SAE & 802.1x|
Encryption| RC4| TKIP| AES counter mode| AES counter mode|
Data Integrity| N/A| MIC| CBC-MAC| GMAC|
Session-Key Size| 
Cipher Type|
Key Management|
