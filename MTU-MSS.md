# Maximum Transmission Unit & Maximum Segment Size


```text
                    <--------------------------- IP MTU ------------------->
                    <--------------------- Ethernet MTU ------------------->
                                               <--------- TCP MSS --------->
       +------------+------------+-------------+---------------------------+------------+
FRAME  | Eth-Header |  IP-Header |  TCP-Header |           Data            | Eth-trailer|
       +------------+------------+-------------+---------------------------+------------+
BYTES        14           20            20                 1460                  4


MSS - Maximum Segment Size
```
### Data Size

Data           | Minimum Size | Maxmum Size | Reasons
---------------|--------------|-------------|----------|
IPv4 packet    | 20 Bytes | 65535 Bytes | 16-bit *Total Length* field in the IPv4 header can store values from 0 to 65535. Since IP header is 20 bytes which defines the minimal. |
Ethernet frame | 64 Bytes | 1518 Bytes  | In the original Ethernet standard (10 Mbps), the minimal ethernet frame size is 64 bytes and Etherent MTU is 1500 bytes. |
