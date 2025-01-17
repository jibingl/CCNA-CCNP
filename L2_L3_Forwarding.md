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
