# IPv4 Subnetting
Keywords: Default classes & their default Network Mask, CIDR/network Mask, octets format, Network Portion, Host Portion, start point of default class.

### 192.168.100.115/27
1. What is the default class this network belongs to? ` C `
2. Working on CIDR (network mask) to collection those below information.
   - How many bits are used for the *Network Portion*? ` 27 `
   - How many bits are used for the *Host Portion*? ` 32 - 27 = 5 `
   - How many bits are used for the *Network Portion* within the 4th octet? ` 27 - 24 = 3 `
   - How manu bits are used for the *Host Portion* within the 4th octet? ` 8 - 3 = 5 `
   - How many ip addresses within each subnet? ` 2 ^ 5 = 32 `
   - What is the increment to each subnet? ` 2 ^ 5 = 32 `
3. Writing down the subnets created by following below rules.
   - Starting from 0 of the default class;
   - Inreasing by 32 (IP addresses within each subnet) to get each subnet's start point;
   ```
   1st subnet: 192.168.100.0
   2nd subnet: 192.168.100.32
   3rd subnet: 192.168.100.64
   4th subnet: 192.168.100.96         <--- This is your target subnet because the given network IP address falls into the range.
   5th subnet: 192.168.100.128
   6th subnet: 192.168.100.160
   ...
   ```
### 172.31.211.201/18
1. What is the default class this network belongs to? ` B `
2. Working on CIDR (network mask) to collection those below information.
   - How many bits are used for the *Network portion*? ` 18 `
   - How many bits are used for the *Host portion*? ` 32 - 18 = 14 `
   - How many bits are used for the *Network portion* within the 3th octet? ` 18 - 16 = 2 `
   - How manu bits are used for the *Host portion* within the 3th octet? ` 8 - 2 = 6 `
   - How many ip addresses within each subnet? ` 2 ^ 14 = 16384 `
   - What is the increment for each subnet? ` 2 ^ 6 = 64 `
3. Writing down the subnets created by following below rules.
   - Starting from 0 of the default class;
   - Inreasing by 64 (the increment to each subnet) to get each subnet's start point;
   ```
   1st subnet: 172.31.0.0
   2nd subnet: 172.31.64.0
   3rd subnet: 172.31.128.0
   4th subnet: 172.31.192.0         <--- This is your target subnet because the given network IP address falls into the range.
   5th subnet: 172.31.256.0         <--- It is not possible that this subnet exsits due to the decimal number of 3rd octet beyonds 255.
   ```

   ### 10.10.10.10/14
1. What is the default class this network belongs to? ` A `
2. Working on CIDR (network mask) to collection those below information.
   - How many bits are used for the *Network portion*? ` 14 `
   - How many bits are used for the *Host portion*? ` 32 - 14 = 18 `
   - How many bits are used for the *Network portion* within the 2th octet? ` 14 - 8 = 6 `
   - How manu bits are used for the *Host portion* within the 2th octet? ` 8 - 6 = 2 `
   - How many ip addresses within each subnet? ` 2 ^ 18 = 262144 `
   - What is the increment for each subnet? ` 2 ^ 2 = 4 `
3. Writing down the subnets created by following below rules.
   - Starting from 0 of the default class;
   - Inreasing by 4 (the increment to each subnet) to get each subnet's start point;
   ```
   1st subnet: 10.0.0.0
   2nd subnet: 10.4.0.0
   3rd subnet: 10.8.0.0         <--- This is your target subnet because the given network IP address falls into the range.
   4th subnet: 10.12.0.0
   5th subnet: 10.16.0.0
   ...
   Last subnet: 10.248.0.0
   ```
