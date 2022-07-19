# Network Time Protocol
  --designed by David L. Mills.

## Stratum
NTP uses **stratum** to indicate the distance of a device to the reference clock. In other words, it shows how accurate a device's time is.  
Level | Clocks | AKA names |
------|--------|-----------|
Stratum 0 | Atomic clocks | Reference clock |
Stratum 1 | clocks connect to stratum 0 | Primary time servers |
Stratum 2 | clocks connect to stratum 1 | |
...(omit) |
Stratum 15| the last clocks level | |
Stratum 16| clock is unsynchronized (no-NTP)| |

> Notes: **Stratum 0** is reserved for atomic clocks. NTP servers cannot advertise themselves as **stratum 0**. A stratum field set to 0 in NTP packet indicates an unspecified stratum.  **Stratum 16** means no NTP configured.

## Configuration
`ntp master [stratum-number<1-15>]` set a device as an internal NTP server. By defualt, the stratum-number of NTP master is 8.

`ntp server {ntp-server-ip/hostname}` set a device as a NTP client.
