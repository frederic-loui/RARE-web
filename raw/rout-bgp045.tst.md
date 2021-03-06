# Example: vpls/ldp over ebgp

## **Topology diagram**

![topology](/img/rout-bgp045.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
bridge 1
 rd 1:1
 rt-import 1:1
 rt-export 1:1
 mac-learn
 private-bridge
 exit
!
bridge 2
 rd 1:2
 rt-import 1:2
 rt-export 1:2
 mac-learn
 private-bridge
 exit
!
bridge 3
 rd 1:3
 rt-import 1:3
 rt-export 1:3
 mac-learn
 private-bridge
 exit
!
bridge 4
 rd 1:4
 rt-import 1:4
 rt-export 1:4
 mac-learn
 private-bridge
 exit
!
vrf definition v1
 rd 1:1
 label-mode per-prefix
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
interface bvi1
 no description
 vrf forwarding v1
 ipv4 address 3.3.3.1 255.255.255.252
 no shutdown
 no log-link-change
 exit
!
interface bvi2
 no description
 vrf forwarding v1
 ipv6 address 4444::1 ffff::
 no shutdown
 no log-link-change
 exit
!
interface bvi3
 no description
 vrf forwarding v1
 ipv6 address 3333::1 ffff::
 no shutdown
 no log-link-change
 exit
!
interface bvi4
 no description
 vrf forwarding v1
 ipv4 address 4.4.4.1 255.255.255.252
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.252
 ipv6 address 1234:1::1 ffff:ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
router bgp4 1
 vrf v1
 local-as 1
 router-id 4.4.4.1
 address-family vpls
 neighbor 2.2.2.2 remote-as 2
 no neighbor 2.2.2.2 description
 neighbor 2.2.2.2 local-as 1
 neighbor 2.2.2.2 address-family vpls
 neighbor 2.2.2.2 distance 20
 neighbor 2.2.2.2 update-source loopback0
 neighbor 2.2.2.2 send-community standard extended
 afi-vpls 1:1 bridge-group 1
 afi-vpls 1:1 update-source loopback0
 afi-vpls 1:2 bridge-group 3
 afi-vpls 1:2 update-source loopback0
 exit
!
router bgp6 1
 vrf v1
 local-as 1
 router-id 6.6.6.1
 address-family vpls
 neighbor 4321::2 remote-as 2
 no neighbor 4321::2 description
 neighbor 4321::2 local-as 1
 neighbor 4321::2 address-family vpls
 neighbor 4321::2 distance 20
 neighbor 4321::2 update-source loopback0
 neighbor 4321::2 send-community standard extended
 afi-vpls 1:1 bridge-group 2
 afi-vpls 1:1 update-source loopback0
 afi-vpls 1:2 bridge-group 4
 afi-vpls 1:2 update-source loopback0
 exit
!
!
ipv4 route v1 2.2.2.2 255.255.255.255 1.1.1.2
!
ipv6 route v1 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::2
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
bridge 1
 rd 1:1
 rt-import 1:1
 rt-export 1:1
 mac-learn
 private-bridge
 exit
!
bridge 2
 rd 1:2
 rt-import 1:2
 rt-export 1:2
 mac-learn
 private-bridge
 exit
!
bridge 3
 rd 1:3
 rt-import 1:3
 rt-export 1:3
 mac-learn
 private-bridge
 exit
!
bridge 4
 rd 1:4
 rt-import 1:4
 rt-export 1:4
 mac-learn
 private-bridge
 exit
!
vrf definition v1
 rd 1:1
 label-mode per-prefix
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
interface bvi1
 no description
 vrf forwarding v1
 ipv4 address 3.3.3.2 255.255.255.252
 no shutdown
 no log-link-change
 exit
!
interface bvi2
 no description
 vrf forwarding v1
 ipv6 address 4444::2 ffff::
 no shutdown
 no log-link-change
 exit
!
interface bvi3
 no description
 vrf forwarding v1
 ipv6 address 3333::2 ffff::
 no shutdown
 no log-link-change
 exit
!
interface bvi4
 no description
 vrf forwarding v1
 ipv4 address 4.4.4.2 255.255.255.252
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.252
 ipv6 address 1234:1::2 ffff:ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
router bgp4 1
 vrf v1
 local-as 2
 router-id 4.4.4.2
 address-family vpls
 neighbor 2.2.2.1 remote-as 1
 no neighbor 2.2.2.1 description
 neighbor 2.2.2.1 local-as 2
 neighbor 2.2.2.1 address-family vpls
 neighbor 2.2.2.1 distance 20
 neighbor 2.2.2.1 update-source loopback0
 neighbor 2.2.2.1 send-community standard extended
 afi-vpls 1:1 bridge-group 1
 afi-vpls 1:1 update-source loopback0
 afi-vpls 1:2 bridge-group 3
 afi-vpls 1:2 update-source loopback0
 exit
!
router bgp6 1
 vrf v1
 local-as 2
 router-id 6.6.6.2
 address-family vpls
 neighbor 4321::1 remote-as 1
 no neighbor 4321::1 description
 neighbor 4321::1 local-as 2
 neighbor 4321::1 address-family vpls
 neighbor 4321::1 distance 20
 neighbor 4321::1 update-source loopback0
 neighbor 4321::1 send-community standard extended
 afi-vpls 1:1 bridge-group 2
 afi-vpls 1:1 update-source loopback0
 afi-vpls 1:2 bridge-group 4
 afi-vpls 1:2 update-source loopback0
 exit
!
!
ipv4 route v1 2.2.2.1 255.255.255.255 1.1.1.1
!
ipv6 route v1 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::1
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
r1#show ipv4 bgp 1 sum
r1#show ipv4 bgp 1 sum
 |~~~~|~~~~~~|~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | as | afis | ready | neighbor | uptime   |
 |----|------|-------|----------|----------|
 | 2  | vpls | true  | 2.2.2.2  | 00:00:28 |
 |____|______|_______|__________|__________|
r1#
r1#
```

```
r1#
r1#
r1#show ipv6 bgp 1 sum
r1#show ipv6 bgp 1 sum
 |~~~~|~~~~~~|~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | as | afis | ready | neighbor | uptime   |
 |----|------|-------|----------|----------|
 | 2  | vpls | true  | 4321::2  | 00:00:27 |
 |____|______|_______|__________|__________|
r1#
r1#
```

```
r1#
r1#
r1#show ipv4 bgp 1 vpls dat
r1#show ipv4 bgp 1 vpls dat
 |~~~~~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~~~|~~~~~~~~|
 | prefix         | hop     | metric     | aspath |
 |----------------|---------|------------|--------|
 | 2.2.2.1/32 1:1 | null    | 0/0/0/0    |        |
 | 2.2.2.2/32 1:1 | 2.2.2.2 | 20/100/0/0 | 2      |
 | 2.2.2.1/32 1:3 | null    | 0/0/0/0    |        |
 | 2.2.2.2/32 1:3 | 2.2.2.2 | 20/100/0/0 | 2      |
 |________________|_________|____________|________|
r1#
r1#
```

```
r1#
r1#
r1#show ipv6 bgp 1 vpls dat
r1#show ipv6 bgp 1 vpls dat
 |~~~~~~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~~~|~~~~~~~~|
 | prefix          | hop     | metric     | aspath |
 |-----------------|---------|------------|--------|
 | 4321::1/128 1:2 | null    | 0/0/0/0    |        |
 | 4321::2/128 1:2 | 4321::2 | 20/100/0/0 | 2      |
 | 4321::1/128 1:4 | null    | 0/0/0/0    |        |
 | 4321::2/128 1:4 | 4321::2 | 20/100/0/0 | 2      |
 |_________________|_________|____________|________|
r1#
r1#
```

```
r1#
r1#
r1#show ipv4 route v1
r1#show ipv4 route v1
 |~~~~~|~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix     | metric | iface     | hop     | time     |
 |-----|------------|--------|-----------|---------|----------|
 | C   | 1.1.1.0/30 | 0/0    | ethernet1 | null    | 00:00:27 |
 | LOC | 1.1.1.1/32 | 0/1    | ethernet1 | null    | 00:00:27 |
 | C   | 2.2.2.1/32 | 0/0    | loopback0 | null    | 00:00:27 |
 | S   | 2.2.2.2/32 | 1/0    | ethernet1 | 1.1.1.2 | 00:00:27 |
 | C   | 3.3.3.0/30 | 0/0    | bvi1      | null    | 00:00:27 |
 | LOC | 3.3.3.1/32 | 0/1    | bvi1      | null    | 00:00:27 |
 | C   | 4.4.4.0/30 | 0/0    | bvi4      | null    | 00:00:27 |
 | LOC | 4.4.4.1/32 | 0/1    | bvi4      | null    | 00:00:27 |
 |_____|____________|________|___________|_________|__________|
r1#
r1#
```

```
r1#
r1#
r1#show ipv6 route v1
r1#show ipv6 route v1
 |~~~~~|~~~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix        | metric | iface     | hop       | time     |
 |-----|---------------|--------|-----------|-----------|----------|
 | C   | 1234:1::/32   | 0/0    | ethernet1 | null      | 00:00:32 |
 | LOC | 1234:1::1/128 | 0/1    | ethernet1 | null      | 00:00:32 |
 | C   | 3333::/16     | 0/0    | bvi3      | null      | 00:00:32 |
 | LOC | 3333::1/128   | 0/1    | bvi3      | null      | 00:00:32 |
 | C   | 4321::1/128   | 0/0    | loopback0 | null      | 00:00:32 |
 | S   | 4321::2/128   | 1/0    | ethernet1 | 1234:1::2 | 00:00:32 |
 | C   | 4444::/16     | 0/0    | bvi2      | null      | 00:00:32 |
 | LOC | 4444::1/128   | 0/1    | bvi2      | null      | 00:00:32 |
 |_____|_______________|________|___________|___________|__________|
r1#
r1#
```

```
r1#
r1#
r1#show bridge 1
r1#show bridge 1
 |~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | interface                    | forward | physical | tx   | rx   | drop |
 |------------------------------|---------|----------|------|------|------|
 | bvi                          | true    | true     | 0    | 0    | 0    |
 | pwe 2.2.2.2 2814754062073857 | true    | false    | 1706 | 1526 | 0    |
 |______________________________|_________|__________|______|______|______|
 |~~~~~~~~~~~~~~~~|~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | address        | interface                    | time     | tx   | rx   | drop |
 |----------------|------------------------------|----------|------|------|------|
 | 0049.411d.7142 | pwe 2.2.2.2 2814754062073857 | 00:00:02 | 1316 | 1346 | 0    |
 | 0053.5111.4552 | bvi                          | 00:00:02 | 1346 | 1946 | 0    |
 |________________|______________________________|__________|______|______|______|
r1#
r1#
```

```
r1#
r1#
r1#show bridge 2
r1#show bridge 2
 |~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | interface                    | forward | physical | tx   | rx   | drop |
 |------------------------------|---------|----------|------|------|------|
 | bvi                          | true    | true     | 0    | 0    | 0    |
 | pwe 4321::2 2814754062073857 | true    | false    | 1744 | 1810 | 0    |
 |______________________________|_________|__________|______|______|______|
 |~~~~~~~~~~~~~~~~|~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | address        | interface                    | time     | tx   | rx   | drop |
 |----------------|------------------------------|----------|------|------|------|
 | 004c.1a72.2776 | bvi                          | 00:00:02 | 1556 | 1998 | 0    |
 | 0058.233d.711f | pwe 4321::2 2814754062073857 | 00:00:02 | 1670 | 1630 | 0    |
 |________________|______________________________|__________|______|______|______|
r1#
r1#
```

```
r1#
r1#
r1#show bridge 3
r1#show bridge 3
 |~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | interface                    | forward | physical | tx   | rx   | drop |
 |------------------------------|---------|----------|------|------|------|
 | bvi                          | true    | true     | 0    | 0    | 0    |
 | pwe 2.2.2.2 2814754062073858 | true    | false    | 1744 | 1810 | 0    |
 |______________________________|_________|__________|______|______|______|
 |~~~~~~~~~~~~~~~~|~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | address        | interface                    | time     | tx   | rx   | drop |
 |----------------|------------------------------|----------|------|------|------|
 | 0070.0709.4a6a | pwe 2.2.2.2 2814754062073858 | 00:00:02 | 1670 | 1630 | 0    |
 | 0071.5c00.335e | bvi                          | 00:00:02 | 1556 | 1998 | 0    |
 |________________|______________________________|__________|______|______|______|
r1#
r1#
```

```
r1#
r1#
r1#show bridge 4
r1#show bridge 4
 |~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | interface                    | forward | physical | tx   | rx   | drop |
 |------------------------------|---------|----------|------|------|------|
 | bvi                          | true    | true     | 0    | 0    | 0    |
 | pwe 4321::2 2814754062073858 | true    | false    | 1346 | 1526 | 0    |
 |______________________________|_________|__________|______|______|______|
 |~~~~~~~~~~~~~~~~|~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~|~~~~~~~~~~|~~~~~~|~~~~~~|~~~~~~|
 | address        | interface                    | time     | tx   | rx   | drop |
 |----------------|------------------------------|----------|------|------|------|
 | 002a.577e.5858 | bvi                          | 00:00:02 | 1346 | 1346 | 0    |
 | 0042.585f.463a | pwe 4321::2 2814754062073858 | 00:00:02 | 1316 | 1346 | 0    |
 |________________|______________________________|__________|______|______|______|
r1#
r1#
```
