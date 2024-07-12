# Troubleshooting Error Disabled Ports

## Causes of Errdisable
Possible Reasons | Practical Examples |
-----------------|--------------------|
Duplex mismatch
Port channel misconfiguration
BPDU guard violation | Connect a new switch to a port with `bpdu guard` enabled
UniDirectional Link Detection (UDLD) condition
Late-collision detection
Link-flap detection
Security violation
Port Aggregation Protocol (PAgP) flap
Layer 2 Tunneling Protocol (L2TP) guard
DHCP snooping rate-limit
Incorrect GBIC / Small Form-Factor Pluggable (SFP) module or cable
Address Resolution Protocol (ARP) inspection
Inline power

## Commands for Errdisable
Categories | Usages | Commands Example |
-----------|-------|------------------|
Troubleshoot | Check ports status for errdisable | `show interfaces status` |
Troubleshoot | Check the reason caused the errdiable on a port | `show interfaces g1/0/1 status err-disabled` |
Configure | Display errdisable settings | `show errdisable detect` |
Configure | Disable error-disable detection | `no errdisable detect cause` |
