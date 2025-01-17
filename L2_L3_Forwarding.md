# Forwarding of Layer2 vs Layer 3

## L2 vs L3 Forwarding
 Layers | Match Rules         | What's done at the layer? |
--------|---------------------|---------------------------|
Layer 2 | Exact match         | Forwarding                |
Layer 3 | Most specific match | Decrement TTP; recompute IP header checksum; change MACs; recompute ethernet FCS; </br> Forwarding | 

#### Most Specific Match
![image](https://github.com/user-attachments/assets/012ee54b-dc89-4bd3-934b-618bd1da5fe1)


## Forwarding Types of Routers
Type           | Arch of control&data planes | Control plane | Data plane |
---------------|-----------------------------|---------------|------------|
Software-based | Shared                      | CPU           | CPU        |
Hardware-based | Separate                    | CPU           | ASIC       |
Hybrid         | Separate                    | CPU           | NP (Network Processor) |

**ASICs with TCAM memory is used as hardware in routers**

## L3 Forwarding Methods

1. Process Switching
![image](https://github.com/user-attachments/assets/c96b6d4b-2adc-44ac-8bd9-e803c8e11598)

2. Fast Switching
![image](https://github.com/user-attachments/assets/3239bf67-f40f-4582-87a2-e576b8fd1cfd)

3. CEF (Cisco Express Forwarding)
![image](https://github.com/user-attachments/assets/a9a3aba1-3f99-41ca-8b83-a9a3a82454ee)
```
R1# show ip cef
!
Prefix               Next Hop
0.0.0.0/0            no route                                              //Defualt route not configured
0.0.0.0/8            drop                                                  //Drop this reserved range
0.0.0.0/32           receive                                               //
1.1.1.1/32           192.168.12.2    GigabitEthernet0/0
                     192.168.13.3    GigabitEthernet0/1
127.0.0.0/8          drop                                                  //Drop this reserved lookback range
192.168.12.0/24      attached        GigabitEthernet0/0                    //Network directly connected to the router
192.168.12.0/32      receive         GigabitEthernet0/0
192.168.12.1/32      receive         GigabitEthernet0/0                    //IP configured on the router's interface
192.168.12.2/32      attached        GigabitEthernet0/0                    //Network directly connected to the router
192.168.12.255/32    receive         GigabitEthernet0/0                    //Broadcast
192.168.13.0/24      attached        GigabitEthernet0/1
192.168.13.0/32      receive         GigabitEthernet0/1
192.168.13.1/32      receive         GigabitEthernet0/1                    //IP configured on the router's interface
192.168.13.3/32      attached        GigabitEthernet0/1
192.168.13.255/32    receive         GigabitEthernet0/1                    //Broadcast
192.168.24.0/24      192.168.12.2    GigabitEthernet0/0
224.0.0.0/4          drop                                                  //Drop this reserved Class D
224.0.0.0/24         receive
240.0.0.0/4          drop                                                  //Drop this reserved CLass E
255.255.255.255/32   receive
```
