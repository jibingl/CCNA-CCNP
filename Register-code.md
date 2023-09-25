# Register Code of Cisco Switches
The register code of a Cisco switch is an 16-bit value used to control the switch behavior during booting process.

### Common Values

Values | Boot From | Boot into | Notes |
-------|-----------|-----------|-------|
0x2102 | Image stored in flash memory | Configuration mode | Default value |
0x2142 | *None* | ROM monitor mode | Bypass startup configuration | 
0x2100 | 1st image found in flash memory | Configuration mode |
0x2101 | 2nd image found in flash memory | Configuration mode |
0x210F | A TFTP server | Configuration mode |
0x2111 | A network boot program (such as BOOTP or DHCP) | Configuration mode |

### How to Config
From congifuration mode:  
```
SW1(config)#config-register 0x2100
SW1(config)#end
SW1#show version
SW1#write memory
```
From ROM monitor mode:  
This is a way to baypass devices authentication. Normally it is for reset the forgotten passwords.
```
rommon1>confreg 0x2142
rommon1>set
rommon1>reset
```
