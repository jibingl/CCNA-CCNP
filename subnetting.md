# IPv4 Subnetting
First thing first, knowing below glossaries. THe last two terms are created/defined by myself.
- CIDR/Network Mask: A number indicates how many bits out of 32-bit are used to present network part.
- Network Portion: The bits used to present network part in a network ip address space.
- Host Portion: The bits used to present host part in a network ip address space.
- Divided-octet: The octet contains both *Network Portion* and *Host Portion*. 
- Increment-number: A customed number used to decide the start point to each subnet. 

### 192.168.100.115/27
1. Working on CIDR/Network Mask to collection below information.
   - How many bits are used for the *Network portion*? ` 27 `
   - How many bits are used for the *Host portion*? ` 32 - 27 = 5 `
   - What is the *divided-octet*? ` 4th `
   - How many bits are used for the *Network portion* within the *divided-octet*? ` 27 - 24 = 3 `
   - How many bits are used for the *Host portion* within the *divided-octet*? ` 8 - 3 = 5 `
   - What is the *increment-number* to each subnet? ` 2 ^ 5 = 32 `
2. Writing down the subnets created by following below rules.
   - Copying the given IP address;
   - Setting the *divided-octet* (It's 4th octet in this case) and all afterwards to 0 so that you have the start point;
   - Inreasing the *divided-octet* by the *increment-number* (It's 32 in this case) to get each subnet's 1st IP;
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
   - How many bits are used for the *Host portion* within the *divided-octet*? ` 8 - 2 = 6 `
   - What is the *increment-number* for each subnet? ` 2 ^ 6 = 64 `
2. Writing down the subnets created by following below rules.
   - Copying the given IP address;
   - Setting the *divided-octet* (It's 3rd here) and all afterwards to 0;
   - Inreasing the *divided-octet* by the *increment-number* (It's 64 here) to get each subnet's 1st IP;
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
   - How many bits are used for the *Host portion* within the *divided-octet*? ` 8 - 6 = 2 `
   - What is the *increment-number* for each subnet? ` 2 ^ 2 = 4 `
2. Writing down the subnets created by following below rules.
   - Copying the given IP address;
   - Setting the *divided-octet* (It's 2nd here) and all afterwards to 0;
   - Inreasing the *divided-octet* by the *increment-number* (It's 4 here) to get each subnet's 1st IP;
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


## Basic Formulas
How many IP addresses totally? ` 2 ^ h ` (h equals to the *Host portion* bits)  
How many usable/assignable IP addresses/hosts? ` 2 ^ h - 2 ` (h equals to the *Host portion* bits)  
What is the network IP address? ` The start point or 1st IP address`  
What is the broadcast IP address? ` The last IP address`  
How many subnet created? ` 2 ^ n ` (n equals to the *Network portion* bits within the *divided-octet*)  
What the x-th subnet? ` (x - 1) * increment-number `  

## Magic Table
Octet | bit 8    | bit 7    | bit 6   | bit 5   | bit 4   | bit 3  | bit 2  | bit 1  |
------|----------|----------|---------|---------|---------|--------|--------|--------| 
4th   | 128      | 64       | 32      | 16      | 8       | 4      | 2      | 1      |
3rd   | 32768    | 16384    | 8192    | 4096    | 2048    | 1024   | 512    | 256    |
2nd   | 8388608  | 4194304  | 2097152 | 1048576 | 524288  | 262144 | 131072 | 65536  |
1st   |2147483648|1073741824|536870912|268435456|134217728|67108864|33554432|16777216|