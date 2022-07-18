# Switch Port Security
Only work for access port on layer 2 through managing MAC addresses that connect to a switch port.

## Deafult Settings and Behavior
Maxmul MAC| Violation mode| Port behavior| Port-recover|
----------|---------------|--------------|-------------|
1 | shutdown | errdisable | manully |

## Violation Modes
Violation mode| Port disabled| Data drop| Syslog| Display error| Increase Violation Counter|
--------------|--------------|----------|-------|--------------|---------------------------|
**shutdown**| Yes| Yes| No| No| Yes|
**restrict**| Yes| Yes| Yes| No| Yes|
**protect**| No| Yes| No| No| No|

## Recommand Configurations
Maximul MAC| Violation mode| Port behavior| Port-recover|
-----------|---------------|--------------|-------------|
2 | shutdown | errdiable | automatically (`errdiable recovery`)|
