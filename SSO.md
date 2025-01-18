# SSO, NSF, GR and NSR
- SSO - Stateful Switchover, maintians interfaces and L2 protocol states.
- NSF - Non-stop Farwarding, checkpoints FIB from active to standby RP to maintain local L3 package forwarding.
- GR - Graceful Restart, allows neigbours to continue forwarding traffic to the device under an RP failover.
- NSR - Non-stop Routing, maintains neighbors adjacencies during RP failover.

All above technologies are used to keep the Data Plane working during a failure of the Control Plane.  

RP Failover               | SSO | SSO + NSF | SSO + GR | NSR |
--------------------------|:---:|:---------:|:--------:|:---:|
Snyc *Start-config*       | Yes | Yes       | Yes      | Yes |
Snyc *Running-config*     | Yes | Yes       | Yes      | Yes |
Keep *L2 protocol states* | Yes | Yes       | Yes      | Yes |
Keep *L3 adjacencies*     | No  | No        | Yes      | Yes |
Keep *L2 forwarding*      | Yes | Yes       | Yes      | Yes |
Keep *L3 forwarding*      | No  | Yes       | Yes      | Yes |


```
R1(config)# redundancy
R1(config-red)# mode sso
```

## SSO HSRP
**SSO HSRP** alters the default behavior of HRSP which allows a standby router to take over the active role, instead of a standby RP becoming active.  
