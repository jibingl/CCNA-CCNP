# Qaulity of Service

```
TRUST BOUNDARY                CLASSIFICATION  POLICING    QUEUING          SCHEDULING                SHAPING
     |                                        +-rate-+     (FIFO)
     |                      - Vo Vo Vo Vo Vo  |      | -- Vo3 Vo2 Vo1 -----LLQ/SPQ--10%--------.
     |                    / - Vi Vi Vi Vi Vi  | Drop | -- Vi3 Vi2 Vi1 -----CBWFQ----20%---|--\  \    +--rate---+
     | Ingress--Routing---- - Vis Vis Vis Vis |  or  | -- Vis3 Vis2 Vis1 --CBWFQ----15%---|----------| Queuing |---Egress
     |                    \ - Hd Hd Hd Hd Hd  |Remark| -- Hd3 Hd2 Hd1 -----CBWFQ----15%---|--/       +---------+
     |                      - Da Da Da Da Da  |      | -- Da3 Da2 Da1 -----CBWFQ----40%---|/  
                                              +------+     (WRED)                        round-robin
```
- Any packets arriving from outside the _trust boundary_ should have any QoS marking ignored and cleared.

## Classification
Classification gives priority to certain types of traffic over others.  
Ways to classifying traffic:   
- #1 ACLs;  
- #2 NBAR (Network Based Application Recognition) inspects layer 3, layer 4, and up to layer 7.  
- #3 **PCP/CoS** (Priority Code Point/Class of Service) of 802.1q tag (layer 2).  

    PCP-value | Traffic-types
    ----------|--------------
    0 (000)   | Best effort (default)
    1 (001)   | Backgroud
    2 (002)   | Excellent effort
    3 (003)   | Critical applications
    4 (004)   | Video
    5 (005)   | Voice
    6 (006)   | Internetwork control
    7 (007)   | Network control

- #4 **DSCP** (Differentiated Service Code Point) of IP header (layer 3).  

    Traffic-types |DSCP-value |Description
    --------------|-----------|-----------
    DF(Defualt Forwarding)  |  0 (000000)        |  Best effort
    EF(Expedited Forwarding)|  46 (101110)       |  Low loss/latency/jitter; Voice traffic
    AF(Assured Forwarding)  |  12-values (bbbbb0)|  4-classe with 3-level drop-precedences; Interactive video AF4x; Streaming video AF3x; High priority data AF2x
    CS(Class Selector)      |  8-values (bbb000) |  Backward compatible to IPP

                       Lowest---drop_precedence--Highest       Value-Converting Diagrams
              Highest  ---------------------------------       0d-  32 16 8  4  2  1   (DSCP-cal-values)
                 |       AF41(34)   AF42(36)   AF43(38)            +--+--+--+--+--+--+        |
              priority   AF31(26)   AF32(28)   AF33(30)        0b- |x  x  x |y  y  0 | (formula 8x + 2y)
                 |       AF21(18)   AF22(20)   AF23(22)            +--+--+--+--+--+--+        |
              Lowest     AF11(10)   AF12(12)   AF13(14)        0d-  4  2  1  2  1      (AF-cal-values)

                       Lowest-----precedence-----Highest
              -----    ---------------------------------       0d-  32 16 8  4  2  1 (DSCP-cal-values)
              IPP       0   1   2   3   4   5   6   7          0b-  x  x  x  0  0  0 (formula 8x)
              CS        CS0 CS1 CS2 CS3 CS4 CS5 CS6 CS7        0d-  4  2  1          (CS-cal-values)
              DSCP      0   8   16  24  32  40  48  56

## Queuing           
When a device receives messages faster than forwards them out, new messages are placed in a queue and taken by a _Scheduler_ later to transmit.  
Classified packets are queued in different queues.  
By default, queued messages will be forwarded in **_FIFO (First In First Out)_** manner.  
When a queue is full, new arriving packets will be dropped. This is called **_tail drop_**.  

                         Queue (FIFO)
               Dropped   +----(queue is full)-+
              +--+ +--+  | +--+--+--+--+--+--+|
              |Pa| |Pa|  | |Pa|Pa|Pa|Pa|Pa|Pa||
              |ck| |ck|  | |ck|ck|ck|ck|ck|ck||
              |et| |et|  | |et|et|et|et|et|et||
              +--+ +--+  | +--+--+--+--+--+--+|
                         +--------------------+
                      (Tail Drop)
                      
**_Tail drop_** is harmful because it can lead to TCP global synchronization.  
**RED (Random Early Detection)** is introduced to prevent tail drop and TCP global synchronization.  
**WRED (Weighted Random Early Detection)** is an improved version, allows to control which packets are dropped depending on the traffic class.  
        
## Scheduling
Theoritecally, a scheduler decides which queue traffic is forwarded from next. A popular scheduling method is _Wieghted Round-Robin_ combining with _**CBWFQ**_.  

    Round-robin - Pakets are taken fom each queue in order cyclically;  
    Weighted    - More data is taken from high priority queues each time the scheduler reaches that queue;  
    CBWFQ       - Class-Based Weighted Fair Queuing, guaranteeing each queuw a certain percentage of the interface's bandwidth.  
                  Not ideal for voice/video because even the highest priority queues have to wait their turn in the scheduler, which adds delay & jitter.
    LLQ         - Low Latency Queue, designating one or more queues as strict priority queues (SPQ);  
                  SPQ queues will be always taken the next packet until it is empty.  
                  Best for voice/video traffic.  
                  
## Policing
Drops or remark packets if the traffic rate goes over the configured rate.   
Solve the downside of LLQ where schedulers are starving if there is always traffic in the designated SPQ.  
ISPs use policing to comply a rate/speed with a WAN service contract.  
    
## Shaping
Buffers traffic in a queue if the traffic rate goes over the configured rate.  
Shapping optimize delay of some traffic by delaying others.   
Companies may deploy shaping on source traffic (custmor-edge-router) to ensure the traffic they send complies with a contract (limited service rate/speed) which may be enfoced in ISPs network by traffic polcing. 

## Qaulity Measurement
Acceptable qaulity| Bandwidth| Delay| Jitter| Loss|
------------------|----------|------|-------|-----|
Audio | 30-128Kbps| one-way delay =< 150ms| jitter =< 30ms| loss =< 1%|
Video | 384K-20Mbps| one-way delay =< 200-400ms| jitter =< 30-50ms| loss =< 0.1-1%|

## Cisco Unified Wireless Network QoS
Class   | Traffic |
--------|---------|
Platinum| Voice   |
Golden  | Video   |
Silver  | Best effort (default)|
Bronze  | Background|
