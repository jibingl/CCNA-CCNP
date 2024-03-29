# DNS Configure on Cisco Router

### Topology
```
                          .---.
  8.8.8.8            .--.(     ).--.                                          PC1
   +--+             (               )              g0/0     g0/1              |.101
   |NS|------------(     Internet    )------------------(+)------------------[=]----.102-PC2
   ====             `-(          )--`  .1          .192  R1 .1                |.103
 dns.googl             `--``----`      192.12.122.0/24       192.168.0.0/24   PC3

```
### Configuration on R1
```
config t
ip dns server
ip host R1 192.168.0.1
ip host PC1 192.168.0.101
ip host PC2 192.168.0.102
ip host PC3 192.168.0.103
ip name-server 8.8.8.8
ip domain lookup
exit
```
### DNS records on R1
```
R1#show host
!
Host             Port   Flags        Age   Type   Address(es)
----             ----   ----------   ---   ----   -----------
youtube.com      None   (temp, OK)   0     IP     172.217.25.110
R1               None   (perm, OK)   4     IP     192.168.0.1
PC1              None   (perm, OK)   1     IP     192.168.0.101
PC2              None   (perm, OK)   4     IP     192.168.0.102
PC3              None   (perm, OK)   4     IP     192.168.0.103
```
