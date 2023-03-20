# Register Code of Cisco Switches
The register code of a Cisco switch is an 16-bit value used to control the switch behavior during booting process.

## Common Values

Values | Boot From | Boot into | Notes |
-------|-----------|-----------|-------|
0x2102 | Image shtored in flash memory | Configuration mode | Default register code |
0x2142 | None | ROM monitor mode | Bypass the startup configuration | 
0x2100 | 1st image found in flash memory | Configuration mode |
0x2101 | 2nd image found in flash memory | Configuration mode |
0x210F | A TFTP server | Configuration mode |
0x2111 | A network boot program (such as BOOTP or DHCP) | Configuration mode |
