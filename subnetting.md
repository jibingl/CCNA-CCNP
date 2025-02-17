# IPv4 Subnetting

## 🥅 Recognization of a Given Network
One of the trickiest aspects of subnetting is identifying the real network ID/address (i.e., the first IP address of a given network). Sometimes, the starting point of the questioned network is not explicitly given. Mistaking the network address results in all further subnetting calculations incorrect.

Below, we’ll guide you through the process of identifying the real network ID using step-by-step examples.

### Example 1: Network 192.168.100.115/27
1. Understanding CIDR/Network Mask  
   The network mask is /27, which provides the following information:
   - **Network portion**: 27 bits
   - **Host portion**: 32 - 27 = 5 bits
     Breakdown of the Host portion represetation in octect format:
     ```
     2^5 = 2^0 . 2^0 . 2^0 . 2^5 = 1 . 1 . 1 . 32
     ```
  
2. Calculating the Network ID.  
   To find the network ID, apply a modulus operation on the given network with the **octec-format** Host portion.  
   Then, subtract the modulus result from the given network.
     ```
           192.168.100.115                  192.168.100.115
      mod)   1.  1.  1. 32              -)    0.  0.  0. 19
      ---------------------    --->     -------------------- 
             0.  0.  0. 19                  192.168.100. 64
     ```
   
3. The Given Network's ID is **192.168.100.64/27** indeed.

### Example 2: Network 10.10.10.10/14
1. Understanding CIDR/Network Mask  
   For the network mask /14, we have:
   - **Network portion**: 14 bits
   - **Host portion**: 32 - 14 = 18 bits
   - **Octec-format Host portion**: 2<sup>18</sup> = 2<sup>0</sup> . 2<sup>2</sup> . 2<sup>8</sup> . 2<sup>8</sup> = 1.4.256.256

2. Calculating the Network ID  
   Apply the modulus and subtraction operations on the given network:
     ```
            10. 10. 10. 10                   10. 10. 10. 10
      mod)   1.  4.256.256              -)    0.  2. 10. 10
      ---------------------    --->     -------------------- 
             0.  2. 10. 10                   10.  8.  0.  0
     ```
3. **Network ID: 10.8.0.0/14**

### Key Formulas or Rules
- **Host portion: h = 32 - n** (where h, n are the number of bits in the Host portion and Network portion respectively)
- **Octect-format Host portion: 2<sup>h1</sup>.2<sup>h2</sup>.2<sup>h3</sup>.2<sup>h4</sup>** (where h1, h2, h3, h4 are the number of host portion bits available in each octect)
     
## 🌑 Subnetting An Given Network 
