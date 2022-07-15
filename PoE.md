# Power over Ethernet

Name | Standard | Walts | Powered Wire Pairs |
---|---|---|---|
Cisco Inline Power (ILP)| not standard | 7 | 2 |
PoE (Type 1) | 802.3af | 15 | 2 |
PoE+ (Type 2) | 802.3at | 30 | 2 |
UPoE (Type 3) | 802.3bt | 60 | 4 |
UPoE+ (Type 4) | 802.3bt | 100 | 4 |

```
            provide-power(DC)
  PD(Phone)-------ethernet---.
                  -           \       power-cable(AC)
  PD(IPcam)-------ethernet----- [=]-----------------------[outlet]
                   -          / PSE
  PD(AP)----------ethernet---`
  (Powered Device)              (Power Sourcing Equipment)
```

_PoE_ has a process to determine if a connected device needs power, and how much power it needs:
 - When a device connects to a PoE-enabled port, the PSE (normally switch) sends low power signals, monitors the response, and determines how much power the PD needs.
 - If the device needs power, the PSE supplies the power to allow the PD to boot.
 - The PSE continues to monitor the PD and supply the required amount of power (but not too much).  

_**Power Policing**_ can be configured to prevent a PD from taking too much power.
 - `power inline police` configures power policing with the default settings: disable the port and send a Syslog message if a PD draws too much power.
 - `power inline police action err-disable` is equivalent to above.
 - `power inline police action log` doesn't shut down the interface instead to restart the interface and send a Syslog message.
