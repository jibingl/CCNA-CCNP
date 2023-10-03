# IPv4 Subnetting
## Glossary
First thing first, knowing below glossaries. The last two terms are customed.
- **CIDR**/**Network Mask**: A number indicates how many bits out of 32-bit are used to present network part.
- **Network Portion**: All bits used to present network part in a given network.
- **Host Portion**: All bits used to present host part in a given network.
- **Divided-octet**: The octet is divided into two parts, one belongs to *Network Portion*, and the other belongs to *Host Portion*. 
- **Increment-number**: A number used to calculate each network/subnet. 

## Part1 - Recognizing An Given Network
Below given networks are examples that demonstrate how to recognize them step by step. 
### 192.168.100.115/27
1. Working on ***CIDR***/***Network Mask*** (27 in this case) to collection information.
   - How many bits are used for the ***Network portion***? ` 27 `
   - How many bits are used for the ***Host portion***? ` 32 - 27 = 5 `
   - What is the ***divided-octet***? ` 4th `
   - How many bits are used for the ***Network portion*** within the ***divided-octet***? ` 27 - 24 = 3 `
   - How many bits are used for the ***Host portion*** within the ***divided-octet***? ` 8 - 3 = 5 `
   - What is the ***increment-number***? ` 2 ^ 5 = 32 `
2. Looking for the given network's range by following below steps.
   - Firstly, copying all octets to the ***divided-octet***'s left; `192.168.100.`
   - Secondly, setting the ***divided-octet*** (4th octet in this case) and all otects to its right to 0; `192.168.100.0`
   - Lastly, constinously inreasing the ***divided-octet***'s values by the ***increment-number*** (32 in this case) untill found the target network range:
   ```
   1st network: 192.168.100.0        #Set the divided-otect (4th octet) and all otects to its right to 0.
   2nd network: 192.168.100.32       #From 1st network, increase 32 on the divided-octect to get 2nd network.
   3rd network: 192.168.100.64       #From 2nd network, increase 32 on the divided-octect to get 3rd network. 
   4th network: 192.168.100.96       #This is target network. Because the given network IP falls into its range (192.168.100.96 - 192.168.100.127).
   5th network: 192.168.100.128
   ...
   ```
### 172.31.211.201/18
1. Working on ***CIDR***/***Network Mask*** (18 here) to collection information.
   - How many bits are used for the ***Network portion***? ` 18 `
   - How many bits are used for the ***Host portion***? ` 32 - 18 = 14 `
   - What is the ***divided-octet***? ` 3th `
   - How many bits are used for the ***Network portion*** within the ***divided-octet***? ` 18 - 16 = 2 `
   - How many bits are used for the ***Host portion*** within the ***divided-octet***? ` 8 - 2 = 6 `
   - What is the ***increment-number***? ` 2 ^ 6 = 64 `
2. Looking for the given network's range by following below steps.
   - Firstly, copying all octets to the ***divided-octet***'s left; `172.31.`
   - Secondly, setting the ***divided-octet*** (3rd octet here) and all otects to its right to 0; `172.31.0.0`
   - Lastly, constinously inreasing the ***divided-octet***'s values by the ***increment-number*** (64 here) untill found the target network range:
   ```
   1st network: 172.31.0.0           #Set the divided-otect (3rd octet) and all otects to its right to 0.
   2nd network: 172.31.64.0          #Range 172.31.64.0 - 172.31.127.255 
   3rd network: 172.31.128.0         #Range 172.31.128.0 - 172.31.191.255
   4th network: 172.31.192.0         #This is your target network. Its range is from 172.31.192.0 to 172.31.255.255
   ```
### 10.10.10.10/14
1. Working on ***CIDR***/***Network Mask*** (14) Mask to collection information.
   - How many bits are used for the ***Network portion***? ` 14 `
   - How many bits are used for the ***Host portion***? ` 32 - 14 = 18 `
   - What is the ***divided-octet***? ` 2th `
   - How many bits are used for the ***Network portion*** within the ***divided-octet***? ` 14 - 8 = 6 `
   - How many bits are used for the ***Host portion*** within the ***divided-octet***? ` 8 - 6 = 2 `
   - What is the ***increment-number***? ` 2 ^ 2 = 4 `
2. Looking for the given network's range.
   - Firstly, copying all octets to the ***divided-octet***'s left; `10.`
   - Secondly, setting the***divided-octet*** (2nd) and all otects to its right to 0; `10.0.0.0`
   - Lastly, constinously inreasing the ***divided-octet***'s values by the ***increment-number*** (4) untill found the target network range:
   ```
   1st network: 10.0.0.0            #10.0.0.0 - 10.3.255.255
   2nd network: 10.4.0.0            #10.4.0.0 - 10.7.255.255
   3rd network: 10.8.0.0            #10.8.0.0 - 10.11.255.255 The target network
   4th network: 10.12.0.0
   ...
   ```
### 10.10.10.10/21
- The ***divided-octet*** is `3rd`.
- The ***increment-number*** is `2 ^ 3 = 8`.
```
   1st network: 10.10.0.0           #10.10.0.0 - 10.10.7.255
   2nd network: 10.10.8.0           #10.10.8.0 - 10.10.15.255 The target network
   3rd network: 10.10.16.0
   ...
```

### Other Basic Formulas and Rules
How many IP addresses totally? ` 2 ^ h ` (h equals to the *Host portion* bits)  
How many usable/assignable IP addresses/hosts? ` 2 ^ h - 2 ` (h equals to the *Host portion* bits)  
What is the network IP address? ` The 1st IP address`  
What is the broadcast IP address? ` The last IP address`  

## Part2 - Subnetting An Given Network 
