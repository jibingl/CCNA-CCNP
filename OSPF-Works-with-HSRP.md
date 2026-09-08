# OSPF Works with HSRP

There are concerns when configuring OSPF along with HSRP.

#### Asymmetrical Routing between OSPF and HSRP  
Upstream OSPF routers may load-balance or prefer the physical interface of the HSRP standby router, causing triangular routing

Solution:  
Adjust OSPF Cost for Symmetrical Routing (Traffic Alignment)
- Hardcode main OSPF route and HSRP active router
- Dynamically adjust OSPF main route to align with HSRP active router accordingly


#### Synchronizing Between OSPF and HSRP  
**Upstream Link Failure**: If an active HSRP router loses its internet/WAN uplink but its local LAN interface stays up, it remains the HSRP active router. Clients keep sending traffic to it, but it cannot forward the traffic because OSPF hasn't failed over the upstream path fast enough or cleanly.

Solution:  
Synchronizing HSRP with OSPF
- HSRP IP Route Tracking or HSRP Object/Interface Tracking
- Configure HSRP Preemption and Delay

**L2 Link Down**: If the Layer 2 (L2) access link on the Active router goes down, the local clients lose connection to that router. However, if the Active router's upstream OSPF link is still up, the upstream core routers will still try to send return traffic to the Active router. Because the Active router no longer has a path down to the clients, it will drop the packets.

Solution: 
- Use HSRP MAC Tracking / Interface Tracking
- Deploy an Inter-Switch "Transit" Link (The Safety Net)


#### Configuring OSPF and HSRP on Same Interface
A common design flaw is deploying OSPF, HSRP, and a Layer 2 clustering technology like Cisco's vPC (Virtual Port-Channel) or Arista's MLAG on the same transit VLAN connecting upstream routers.

Solution:  
Separate Transit Links From Access Layers
- OSPF Passive interface


#### Synchronizing OSPF with HSRP (Not recommend)
----------------------------
synchronize OSPF with HSRP so that whichever router is the HSRP Active gateway automatically becomes the preferred OSPF path for return traffic.
The most elegant, automated way to achieve this is by using a feature called OSPF Cost Customization via Object Tracking. Instead of hardcoding a high OSPF cost on Router B, you make the OSPF cost dynamic.
- Using a syslog message to trigger EEM
- Using an IP SLA Tracking Object to trigger EEM


## Example Configuration

Router A
```
! 1. CONFIGURE TRACKING OBJECTS
track 10 interface GigabitEthernet0/0 line-protocol    ! Tracks upstream link/interface
track 20 ip route 10.0.0.0 255.255.255.0 reachability  ! Tracks upstream route
track 30 ip route 0.0.0.0 0.0.0.0 reachability         ! Tracks upstream default route to Internet/WAN
! Create a combined list that goes DOWN if ANY object inside it fails
track 100 list boolean or
 object 10
 object 20
 object 30
!
! 2. CONFIGURE UPSTREAM INTERFACE (OSPF WAN)
interface GigabitEthernet0/0
 ip address 10.0.0.1 255.255.255.252
 ip ospf 1 area 0
!
! 3. CONFIGURE INTER-SWITCH TRANSIT LINK (OSPF ONLY)
interface GigabitEthernet0/2
 ip address 10.1.1.1 255.255.255.252
 ip ospf 1 area 0
!
! 4. CONFIGURE CLIENT INTERFACE (HSRP ONLY)
interface GigabitEthernet0/1
 ip address 192.168.1.2 255.255.255.0
 !
 standby 1 ip 192.168.1.1                            ! Define Virtual Gateway IP
 standby 1 priority 105                              ! Higher priority to ensure it is Active
 standby 1 preempt delay minimum 60                  ! Wait for OSPF to boot before taking back control
 standby 1 track 100 decrement 20                    ! Drop priority to 85 if upstream interface, route, or default route dies
 standby 1 track GigabitEthernet0/1                  ! If this local interface goes down, HSRP priority drops by 10 (down to 95)
!
! 5. CONFIGURE OSPF ROUTING PROCESS
router ospf 1
 router-id 1.1.1.1
 passive-interface GigabitEthernet0/1                ! BLOCK OSPF packets from flooding client LAN
 network 192.168.1.0 0.0.0.255 area 0                ! Advertise client network upstream
```

Router B
```
! 1. CONFIGURE UPSTREAM INTERFACE (OSPF WAN)
interface GigabitEthernet0/0
 ip address 10.0.0.5 255.255.255.252
 ip ospf 1 area 0
!
! 2. CONFIGURE INTER-SWITCH TRANSIT LINK (OSPF ONLY)
interface GigabitEthernet0/2
 ip address 10.1.1.2 255.255.255.252
 ip ospf 1 area 0
!
! 3. CONFIGURE CLIENT INTERFACE (HSRP ONLY)
interface GigabitEthernet0/1
 ip address 192.168.1.3 255.255.255.0
 !
 standby 1 ip 192.168.1.1                            ! Define same Virtual Gateway IP
 standby 1 priority 100                              ! Default lower priority (Standby status)
 standby 1 preempt                                   ! Preempt immediately if Router A priority drops
!
! 4. CONFIGURE OSPF ROUTING PROCESS
router ospf 1
 router-id 2.2.2.2
 passive-interface GigabitEthernet0/1                ! BLOCK OSPF packets from flooding client LAN
 network 192.168.1.0 0.0.0.255 area 0                ! Advertise client network upstream
```



