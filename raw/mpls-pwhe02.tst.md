# Example: ethernet over packet over udp pwhe

## **Topology diagram**

![topology](/img/mpls-pwhe02.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
vrf definition v1
 rd 1:1
 exit
!
interface loopback0
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.1 255.255.255.255
 ipv6 address 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.0
 ipv6 address 1234::1 ffff::
 no shutdown
 no log-link-change
 exit
!
interface pwether1
 no description
 mtu 1500
 macaddr 0057.051f.4b56
 vrf forwarding v1
 ipv4 address 3.3.3.1 255.255.255.0
 pseudowire v1 loopback0 pckoudp 2.2.2.2 1234
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 2.2.2.2 255.255.255.255 1.1.1.2
!
ipv6 route v1 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234::2
!
!
!
!
!
!
!
!
!
!
!
!
end
```

**r2:**
```
hostname r2
buggy
!
logging file debug ../binTmp/zzz-log-r2.run
!
vrf definition v1
 rd 1:1
 exit
!
interface loopback0
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.2 255.255.255.255
 ipv6 address 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.0
 ipv6 address 1234::2 ffff::
 no shutdown
 no log-link-change
 exit
!
interface pwether1
 no description
 mtu 1500
 macaddr 0050.6c61.7138
 vrf forwarding v1
 ipv4 address 3.3.3.2 255.255.255.0
 pseudowire v1 loopback0 pckoudp 2.2.2.1 1234
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 2.2.2.1 255.255.255.255 1.1.1.1
!
ipv6 route v1 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234::1
!
!
!
!
!
!
!
!
!
!
!
!
end
```

## **Verification**

```
r1#
r1#
r1#show inter pweth1 full
r1#show inter pweth1 full
pwether1 is up (since 00:00:05, 1 changes)
 description:
 type is pwether, hwaddr=0057.051f.4b56, mtu=1500, bw=4000kbps, vrf=v1
 ip4 address=3.3.3.1/24, netmask=255.255.255.0, ifcid=10013
 received 15 packets (1346 bytes) dropped 0 packets (0 bytes)
 transmitted 15 packets (1346 bytes) promisc=false macsec=false
 |~~~~~~~|~~~~~~|~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~~~~|~~~~~~|
 | time  | send | receive | drop | send | receive | drop |
 |-------|------|---------|------|------|---------|------|
 | 1sec  | 376  | 376     | 0    | 4    | 4       | 0    |
 | 1min  | 0    | 0       | 0    | 0    | 0       | 0    |
 | 1hour | 0    | 0       | 0    | 0    | 0       | 0    |
 |_______|______|_________|______|______|_________|______|
 |~~~~~~~~|~~~~~~~|~~~~~~~~~|~~~~|~~~~|~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | type   | value | handler | tx | rx | drop | tx   | rx   | drop |
 |--------|-------|---------|----|----|------|------|------|------|
 | ethtyp | 0800  | ip4     | 14 | 14 | 0    | 1316 | 1316 | 0    |
 | ethtyp | 0806  | arp4    | 1  | 1  | 0    | 30   | 30   | 0    |
 |________|_______|_________|____|____|______|______|______|______|
 |~~~~~|~~~~|~~~~|
 | who | tx | rx |
 |-----|----|----|
 |_____|____|____|
 |~~~~~~~|~~~~~~|~~~~~~|
 | proto | pack | byte |
 |-------|------|------|
 | 1     | 14   | 1316 |
 | 0     | 1    | 30   |
 |_______|______|______|
 |~~~~~~~~~~~~|~~~~|~~~~|~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | size       | tx | rx | drop | tx   | rx   | drop |
 |------------|----|----|------|------|------|------|
 | 0-255      | 15 | 15 | 0    | 1346 | 1346 | 0    |
 | 256-511    | 0  | 0  | 0    | 0    | 0    | 0    |
 | 512-767    | 0  | 0  | 0    | 0    | 0    | 0    |
 | 768-1023   | 0  | 0  | 0    | 0    | 0    | 0    |
 | 1024-1279  | 0  | 0  | 0    | 0    | 0    | 0    |
 | 1280-1535  | 0  | 0  | 0    | 0    | 0    | 0    |
 | 1536-1791  | 0  | 0  | 0    | 0    | 0    | 0    |
 | 1792-65535 | 0  | 0  | 0    | 0    | 0    | 0    |
 |____________|____|____|______|______|______|______|
 |~~~~~~~|~~~~~|~~~~~|~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | class | cos | exp | prec | cos  | exp  | prec |
 |-------|-----|-----|------|------|------|------|
 | 0     | 15  | 15  | 15   | 1346 | 1346 | 1346 |
 | 1     | 0   | 0   | 0    | 0    | 0    | 0    |
 | 2     | 0   | 0   | 0    | 0    | 0    | 0    |
 | 3     | 0   | 0   | 0    | 0    | 0    | 0    |
 | 4     | 0   | 0   | 0    | 0    | 0    | 0    |
 | 5     | 0   | 0   | 0    | 0    | 0    | 0    |
 | 6     | 0   | 0   | 0    | 0    | 0    | 0    |
 | 7     | 0   | 0   | 0    | 0    | 0    | 0    |
 |_______|_____|_____|______|______|______|______|
        6016|
        5414|#
        4812|#
        4211|#
        3609|#
        3008|#
        2406|#
        1804|#
        1203|#
         601|#
           0|##
         bps|0---------10--------20--------30--------40--------50-------- seconds
           1|
           0|
           0|
           0|
           0|
           0|
           0|
           0|
           0|
           0|
           0|
         bps|0---------10--------20--------30--------40--------50-------- minutes
           1|
           0|
           0|
           0|
           0|
           0|
           0|
           0|
           0|
           0|
           0|
         bps|0---------10--------20--------30--------40--------50-------- hours
r1#
r1#
```

```
r1#
r1#
r1#show ipv4 arp pweth1
r1#show ipv4 arp pweth1
 |~~~~~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~|
 | mac            | address | time     | static |
 |----------------|---------|----------|--------|
 | 0050.6c61.7138 | 3.3.3.2 | 00:00:05 | false  |
 |________________|_________|__________|________|
r1#
r1#
```

```
r1#
r1#
r1#show ipv6 neigh pweth1
r1#show ipv6 neigh pweth1
% protocol not enabled
r1#
r1#
```
