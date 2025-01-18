# Output of `show` Commands

📎 Table-1 **`show mac address-table [dynamic | static | vlan <vlan-id> | interface <int-id>]`**
Vlan | Mac Address    | Type    | Ports |
-----|----------------|---------|-------|
ALL  | 0100-0ccc-cccc | STATIC  | CPU   |
1    | 0001-1122-aacc | DYNAMIC | Fa0/1 |

Cisco cmds `show mac address-table`, `clear mac address-table dynamic [address <specific-mac> | interface <inter-id>]`.  
Aging is 5 minutes.

📎 Table-2 **`show arp [vlan <vlan-id>| interface <int-id>]`**
Internet Address | Age |Phtsical Address   | Type | Interface |
-----------------|-----|-------------------|------|-----------|
169.254.255.255  | -   | ff-ff-ff-ff-ff-ff | ARPA |           |
192.168.1.1      | -   | 1122.3344.55aa    | ARPA | Vlan1     |
192.168.1.101    | 3   | 0001.1122.aacc    | ARPA | Vlan1     |
