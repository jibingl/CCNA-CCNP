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


## CEF
**Example Network Topolgy**  
![image](https://github.com/user-attachments/assets/d1095427-14a5-4378-8da6-1dc11f26f36c)

#### 1. FIB(Forwarding Information Base)
Using `show ip cef` to display **FIB** table information  
![image](https://github.com/user-attachments/assets/b725ac1d-b3fb-4677-8fdc-898f9f71253a)

`show ip cef 10.10.23.0 detail`  
![image](https://github.com/user-attachments/assets/adf1ab15-4697-4670-b424-f0f296640171)

#### 2. Adjacency Table
Using `show adjacency detail` to display the **Adjacency Table** detailed information  
![image](https://github.com/user-attachments/assets/52be617a-f021-442c-8a16-12dc44300fa4)
> `AABBCC000200` `AABBCC000100` `0800` represents `DES-MAC` `SRC-MAC` `type`, in our case, it is `R2's E0/0 MAC` `R1's E0/0 MAC` `IPv4`.
