# Example: mp2mp ldp tunnel

## **Topology diagram**

![topology](/img/mpls-ldp15.tst.png)

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
 ipv6 address 1234:1::1 ffff:ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
interface tunnel1
 no description
 tunnel key 1234
 tunnel vrf v1
 tunnel source loopback0
 tunnel destination 2.2.2.2
 tunnel mode mp2mpldp
 vrf forwarding v1
 ipv4 address 3.3.3.1 255.255.255.0
 no shutdown
 no log-link-change
 exit
!
interface tunnel2
 no description
 tunnel key 1234
 tunnel vrf v1
 tunnel source loopback0
 tunnel destination 4321::2
 tunnel mode mp2mpldp
 vrf forwarding v1
 ipv6 address 3333::1 ffff:ffff::
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 0.0.0.0 0.0.0.0 1.1.1.2
!
ipv6 route v1 :: :: 1234:1::2
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
 ipv6 address 1234:1::2 ffff:ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.2.1 255.255.255.0
 ipv6 address 1234:2::1 ffff:ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
interface ethernet3
 no description
 vrf forwarding v1
 ipv4 address 1.1.3.1 255.255.255.0
 ipv6 address 1234:3::1 ffff:ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 2.2.2.1 255.255.255.255 1.1.1.1
ipv4 route v1 2.2.2.3 255.255.255.255 1.1.2.2
ipv4 route v1 2.2.2.4 255.255.255.255 1.1.3.2
!
ipv6 route v1 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::1
ipv6 route v1 4321::3 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:2::2
ipv6 route v1 4321::4 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:3::2
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

**r3:**
```
hostname r3
buggy
!
logging file debug ../binTmp/zzz-log-r3.run
!
vrf definition v1
 rd 1:1
 exit
!
interface loopback0
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.3 255.255.255.255
 ipv6 address 4321::3 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.2.2 255.255.255.0
 ipv6 address 1234:2::2 ffff:ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
interface tunnel1
 no description
 tunnel key 1234
 tunnel vrf v1
 tunnel source loopback0
 tunnel destination 2.2.2.2
 tunnel mode mp2mpldp
 vrf forwarding v1
 ipv4 address 3.3.3.3 255.255.255.0
 no shutdown
 no log-link-change
 exit
!
interface tunnel2
 no description
 tunnel key 1234
 tunnel vrf v1
 tunnel source loopback0
 tunnel destination 4321::2
 tunnel mode mp2mpldp
 vrf forwarding v1
 ipv6 address 3333::3 ffff:ffff::
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 0.0.0.0 0.0.0.0 1.1.2.1
!
ipv6 route v1 :: :: 1234:2::1
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

**r4:**
```
hostname r4
buggy
!
logging file debug ../binTmp/zzz-log-r4.run
!
vrf definition v1
 rd 1:1
 exit
!
interface loopback0
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.4 255.255.255.255
 ipv6 address 4321::4 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.3.2 255.255.255.0
 ipv6 address 1234:3::2 ffff:ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
interface tunnel1
 no description
 tunnel key 1234
 tunnel vrf v1
 tunnel source loopback0
 tunnel destination 2.2.2.2
 tunnel mode mp2mpldp
 vrf forwarding v1
 ipv4 address 3.3.3.4 255.255.255.0
 no shutdown
 no log-link-change
 exit
!
interface tunnel2
 no description
 tunnel key 1234
 tunnel vrf v1
 tunnel source loopback0
 tunnel destination 4321::2
 tunnel mode mp2mpldp
 vrf forwarding v1
 ipv6 address 3333::4 ffff:ffff::
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 0.0.0.0 0.0.0.0 1.1.3.1
!
ipv6 route v1 :: :: 1234:3::1
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
r3#
r3#
r3#show mpls forw
r3#show mpls forw
 |~~~~~~~~|~~~~~~|~~~~~~~|~~~~~~|~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~|
 | label  | vrf  | iface | hop  | label      | targets | bytes   |
 |--------|------|-------|------|------------|---------|---------|
 | 157583 | v1:4 | null  | null | unlabelled | local   | 0       |
 | 378091 | v1:6 | null  | null | unlabelled | local   | 967232  |
 | 397451 | v1:4 | null  | null | unlabelled | local   | 1406864 |
 | 411192 | v1:6 | null  | null | unlabelled | local   | 0       |
 |________|______|_______|______|____________|_________|_________|
r3#
r3#
```

```
r3#
r3#
r3#show ipv4 ldp v1 sum
r3#show ipv4 ldp v1 sum
 |~~~~~~~|~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | learn | advert | l2learn | l2advert | mplearn | mpadvert | neighbor | uptime   |
 |-------|--------|---------|----------|---------|----------|----------|----------|
 | 0     | 0      | 0       | 0        | 1       | 1        | 1.1.2.1  | 00:00:11 |
 |_______|________|_________|__________|_________|__________|__________|__________|
r3#
r3#
```

```
r3#
r3#
r3#show ipv6 ldp v1 sum
r3#show ipv6 ldp v1 sum
 |~~~~~~~|~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
 | learn | advert | l2learn | l2advert | mplearn | mpadvert | neighbor  | uptime   |
 |-------|--------|---------|----------|---------|----------|-----------|----------|
 | 0     | 0      | 0       | 0        | 1       | 1        | 1234:2::1 | 00:00:11 |
 |_______|________|_________|__________|_________|__________|___________|__________|
r3#
r3#
```

```
r3#
r3#
r3#show ipv4 ldp v1 mpdat
r3#show ipv4 ldp v1 mpdat
 |~~~~~~~|~~~~~~~~~|~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|
 | type  | root    | opaque               | uplink  | peers                       |
 |-------|---------|----------------------|---------|-----------------------------|
 | mp2mp | 2.2.2.2 | 01 00 04 00 00 04 d2 | 1.1.2.1 | local 397451/1.1.2.1/170337 |
 |_______|_________|______________________|_________|_____________________________|
r3#
r3#
```

```
r3#
r3#
r3#show ipv6 ldp v1 mpdat
r3#show ipv6 ldp v1 mpdat
 |~~~~~~~|~~~~~~~~~|~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|
 | type  | root    | opaque               | uplink    | peers                         |
 |-------|---------|----------------------|-----------|-------------------------------|
 | mp2mp | 4321::2 | 01 00 04 00 00 04 d2 | 1234:2::1 | local 378091/1234:2::1/753398 |
 |_______|_________|______________________|___________|_______________________________|
r3#
r3#
```

```
r3#
r3#
r3#show inter tun1 full
r3#show inter tun1 full
tunnel1 is up (since 00:00:17, 1 changes)
 description:
 type is mp2mpldp, hwaddr=none, mtu=1500, bw=8000kbps, vrf=v1
 ip4 address=3.3.3.3/24, netmask=255.255.255.0, ifcid=10013
 received 0 packets (0 bytes) dropped 0 packets (0 bytes)
 transmitted 13170 packets (1237980 bytes) promisc=false macsec=false
 |~~~~~~~|~~~~~~~~|~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~~~~|~~~~~~|
 | time  | send   | receive | drop | send | receive | drop |
 |-------|--------|---------|------|------|---------|------|
 | 1sec  | 540218 | 0       | 0    | 5747 | 0       | 0    |
 | 1min  | 0      | 0       | 0    | 0    | 0       | 0    |
 | 1hour | 0      | 0       | 0    | 0    | 0       | 0    |
 |_______|________|_________|______|______|_________|______|
 |~~~~~~~~|~~~~~~~|~~~~~~~~~|~~~~~~~|~~~~|~~~~~~|~~~~~~~~~|~~~~|~~~~~~|
 | type   | value | handler | tx    | rx | drop | tx      | rx | drop |
 |--------|-------|---------|-------|----|------|---------|----|------|
 | ethtyp | 0800  | ip4     | 13173 | 0  | 0    | 1238262 | 0  | 0    |
 |________|_______|_________|_______|____|______|_________|____|______|
 |~~~~~|~~~~|~~~~|
 | who | tx | rx |
 |-----|----|----|
 |_____|____|____|
 |~~~~~~~|~~~~~~~|~~~~~~~~~|
 | proto | pack  | byte    |
 |-------|-------|---------|
 | 1     | 13181 | 1239014 |
 |_______|_______|_________|
 |~~~~~~~~~~~~|~~~~~~~|~~~~|~~~~~~|~~~~~~~~~|~~~~|~~~~~~|
 | size       | tx    | rx | drop | tx      | rx | drop |
 |------------|-------|----|------|---------|----|------|
 | 0-255      | 13181 | 0  | 0    | 1239014 | 0  | 0    |
 | 256-511    | 0     | 0  | 0    | 0       | 0  | 0    |
 | 512-767    | 0     | 0  | 0    | 0       | 0  | 0    |
 | 768-1023   | 0     | 0  | 0    | 0       | 0  | 0    |
 | 1024-1279  | 0     | 0  | 0    | 0       | 0  | 0    |
 | 1280-1535  | 0     | 0  | 0    | 0       | 0  | 0    |
 | 1536-1791  | 0     | 0  | 0    | 0       | 0  | 0    |
 | 1792-65535 | 0     | 0  | 0    | 0       | 0  | 0    |
 |____________|_______|____|______|_________|____|______|
 |~~~~~~~|~~~~~~~|~~~~~~~|~~~~~~~|~~~~~~~~~|~~~~~~~~~|~~~~~~~~~|
 | class | cos   | exp   | prec  | cos     | exp     | prec    |
 |-------|-------|-------|-------|---------|---------|---------|
 | 0     | 13182 | 13182 | 13182 | 1239108 | 1239108 | 1239108 |
 | 1     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 2     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 3     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 4     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 5     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 6     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 7     | 0     | 0     | 0     | 0       | 0       | 0       |
 |_______|_______|_______|_______|_________|_________|_________|
       4321k|
       3889k|#
       3457k|#
       3025k|#
       2593k|##
       2160k|##
       1728k|##
       1296k|###
        864k|###
        432k|###
           0|####
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
r3#
r3#
```

```
r3#
r3#
r3#show inter tun2 full
r3#show inter tun2 full
tunnel2 is up (since 00:00:17, 1 changes)
 description:
 type is mp2mpldp, hwaddr=none, mtu=1500, bw=8000kbps, vrf=v1
 ip6 address=3333::3/32, netmask=ffff:ffff::, ifcid=10013
 received 0 packets (0 bytes) dropped 0 packets (0 bytes)
 transmitted 12675 packets (1444838 bytes) promisc=false macsec=false
 |~~~~~~~|~~~~~~~~|~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~~~~|~~~~~~|
 | time  | send   | receive | drop | send | receive | drop |
 |-------|--------|---------|------|------|---------|------|
 | 1sec  | 655386 | 0       | 0    | 5749 | 0       | 0    |
 | 1min  | 0      | 0       | 0    | 0    | 0       | 0    |
 | 1hour | 0      | 0       | 0    | 0    | 0       | 0    |
 |_______|________|_________|______|______|_________|______|
 |~~~~~~~~|~~~~~~~|~~~~~~~~~|~~~~~~~|~~~~|~~~~~~|~~~~~~~~~|~~~~|~~~~~~|
 | type   | value | handler | tx    | rx | drop | tx      | rx | drop |
 |--------|-------|---------|-------|----|------|---------|----|------|
 | ethtyp | 86dd  | ip6     | 12677 | 0  | 0    | 1445066 | 0  | 0    |
 |________|_______|_________|_______|____|______|_________|____|______|
 |~~~~~|~~~~|~~~~|
 | who | tx | rx |
 |-----|----|----|
 |_____|____|____|
 |~~~~~~~|~~~~~~~|~~~~~~~~~|
 | proto | pack  | byte    |
 |-------|-------|---------|
 | 58    | 12678 | 1445180 |
 |_______|_______|_________|
 |~~~~~~~~~~~~|~~~~~~~|~~~~|~~~~~~|~~~~~~~~~|~~~~|~~~~~~|
 | size       | tx    | rx | drop | tx      | rx | drop |
 |------------|-------|----|------|---------|----|------|
 | 0-255      | 12679 | 0  | 0    | 1445294 | 0  | 0    |
 | 256-511    | 0     | 0  | 0    | 0       | 0  | 0    |
 | 512-767    | 0     | 0  | 0    | 0       | 0  | 0    |
 | 768-1023   | 0     | 0  | 0    | 0       | 0  | 0    |
 | 1024-1279  | 0     | 0  | 0    | 0       | 0  | 0    |
 | 1280-1535  | 0     | 0  | 0    | 0       | 0  | 0    |
 | 1536-1791  | 0     | 0  | 0    | 0       | 0  | 0    |
 | 1792-65535 | 0     | 0  | 0    | 0       | 0  | 0    |
 |____________|_______|____|______|_________|____|______|
 |~~~~~~~|~~~~~~~|~~~~~~~|~~~~~~~|~~~~~~~~~|~~~~~~~~~|~~~~~~~~~|
 | class | cos   | exp   | prec  | cos     | exp     | prec    |
 |-------|-------|-------|-------|---------|---------|---------|
 | 0     | 12681 | 12681 | 12681 | 1445522 | 1445522 | 1445522 |
 | 1     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 2     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 3     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 4     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 5     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 6     | 0     | 0     | 0     | 0       | 0       | 0       |
 | 7     | 0     | 0     | 0     | 0       | 0       | 0       |
 |_______|_______|_______|_______|_________|_________|_________|
       5243k|
       4718k|#
       4194k|#
       3670k|#
       3145k|#
       2621k|##
       2097k|##
       1572k|##
       1048k|##
        524k|##
           0|###             #
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
r3#
r3#
```
