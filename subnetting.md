# IPv4 Subnetting
Firt thing firt, knowing below glossaries. THe last two terms are created/defined by myself.
- CIDR/network Mask: A number indicates how many bits used to present network part within 32 bits.
- Network Portion: All bits used to present network part within a subnet.
- Host Portion: All bits used to present host part within a subnet.
- Divided-octet: The octet contains both *Network Portion* and *Host Portion*. 
- Increment-number: A customed number used to decide the start point to each subnet. 

### 192.168.100.115/27
1. Working on CIDR/Network Mask to collection below information.
   - How many bits are used for the *Network portion*? ` 27 `
   - How many bits are used for the *Host portion*? ` 32 - 27 = 5 `
   - What is the *divided-octet*? ` 4th `
   - How many bits are used for the *Network portion* within the *divided-octet*? ` 27 - 24 = 3 `
   - How manu bits are used for the *Host portion* within the *divided-octet*? ` 8 - 3 = 5 `
   - What is the *increment-number* to each subnet? ` 2 ^ 5 = 32 `
2. Writing down the subnets created by following below rules.
   - Copying the given IP address;
   - Set the *divided-octet* (It's 4th octet in this case) and all afterwards to 0 so that you have the start point;
   - Inreasing by the *increment-number* (It's 32 in this case) to get each subnet's 1st IP;
   ```
   1st subnet: 192.168.100.0          <--- Set the divided-otect (4th octet) to 0.
   2nd subnet: 192.168.100.32
   3rd subnet: 192.168.100.64
   4th subnet: 192.168.100.96         <--- This is your target subnet because the given network IP address falls into the range.
   5th subnet: 192.168.100.128
   6th subnet: 192.168.100.160
   ...
   ```
### 172.31.211.201/18
1. Working on CIDR/Network Mask to collection below information.
   - How many bits are used for the *Network portion*? ` 18 `
   - How many bits are used for the *Host portion*? ` 32 - 18 = 14 `
   - What is the *divided-octet*? ` 3th `
   - How many bits are used for the *Network portion* within the *divided-octet*? ` 18 - 16 = 2 `
   - How manu bits are used for the *Host portion* within the *divided-octet*? ` 8 - 2 = 6 `
   - What is the *increment-number* for each subnet? ` 2 ^ 6 = 64 `
2. Writing down the subnets created by following below rules.
   - Copying the given IP address;
   - Set the *divided-octet* (It's 3rd here) and all afterwards to 0;
   - Inreasing by the *increment-number* (It's 64 here) to get each subnet's 1st IP;
   ```
   1st subnet: 172.31.0.0           <--- Set the divided-otect (3rd octet) and afterwards (4th octet) to 0.
   2nd subnet: 172.31.64.0
   3rd subnet: 172.31.128.0
   4th subnet: 172.31.192.0         <--- This is your target subnet because the given network IP address falls into the range.
   5th subnet: 172.31.256.0         <--- It is not possible that this subnet exsits due to the decimal number of 3rd octet beyonds 255.
   ```
### 10.10.10.10/14
1. Working on CIDR/Network Mask to collection below information.
   - How many bits are used for the *Network portion*? ` 14 `
   - How many bits are used for the *Host portion*? ` 32 - 14 = 18 `
   - What is the *divided-octet*? ` 2th `
   - How many bits are used for the *Network portion* within the *divided-octet*? ` 14 - 8 = 6 `
   - How manu bits are used for the *Host portion* within the *divided-octet*? ` 8 - 6 = 2 `
   - What is the *increment-number* for each subnet? ` 2 ^ 2 = 4 `
2. Writing down the subnets created by following below rules.
   - Copying the given IP address;
   - Set the *divided-octet* (It's 2nd here) and all afterwards to 0;
   - Inreasing by the *increment-number* (It's 4 here) to get each subnet's 1st IP;
   ```
   1st subnet: 10.0.0.0           <--- Set the divided-otect (2nd octet) and afterwards (3rd & 4th octets) to 0.
   2nd subnet: 10.4.0.0
   3rd subnet: 10.8.0.0           <--- This is your target subnet because the given network IP address falls into the range.
   4th subnet: 10.12.0.0
   5th subnet: 10.16.0.0
   ...
   Last subnet: 10.252.0.0
   ```
### 10.10.10.10/21
- The divided-octet is `3rd`.
- The increment-number is `8 = 2 ^ 3`.
```
   1st subnet: 10.10.0.0           <--- Set the divided-otect (3rd octet) and afterwards (4th octet) to 0.
   2nd subnet: 10.10.8.0           <--- This is your target subnet because the given network IP address falls into the range.
   3rd subnet: 10.10.16.0
   4th subnet: 10.10.24.0
   ...
   Last subnet: 10.10.248.0
```


## Basic Formula
How many IP addresses totally? ` 2 ^ m ` (m equals to the *Host portion* bits)  
How many usable/assignable IP addresses/hosts? ` 2 ^ m - 2 ` (m equals to the *Host portion* bits)  
What is the network IP address? ` The start point or 1st IP address`  
What is the broadcast IP address? ` The last IP address`  
