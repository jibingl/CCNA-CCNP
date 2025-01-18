# SSO, NSF, GR and NSR
- SSO - Stateful Switchover
- NSF - Non-stop Farwarding
- GR - Graceful Restart
- NSR - Non-stop Routing

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
