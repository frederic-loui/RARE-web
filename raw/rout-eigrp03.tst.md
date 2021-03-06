# Example: eigrp point2point chain

## **Topology diagram**

![topology](/img/rout-eigrp03.tst.png)

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
router eigrp4 1
 vrf v1
 router-id 4.4.4.1
 as 1
 redistribute connected
 exit
!
router eigrp6 1
 vrf v1
 router-id 6.6.6.1
 as 1
 redistribute connected
 exit
!
interface loopback1
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
 ipv4 address 1.1.1.1 255.255.255.252
 ipv6 address 1234:1::1 ffff:ffff::
 router eigrp4 1 enable
 router eigrp6 1 enable
 no shutdown
 no log-link-change
 exit
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
router eigrp4 1
 vrf v1
 router-id 4.4.4.2
 as 1
 redistribute connected
 exit
!
router eigrp6 1
 vrf v1
 router-id 6.6.6.2
 as 1
 redistribute connected
 exit
!
interface loopback1
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
 ipv4 address 1.1.1.2 255.255.255.252
 ipv6 address 1234:1::2 ffff:ffff::
 router eigrp4 1 enable
 router eigrp6 1 enable
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.5 255.255.255.252
 ipv6 address 1234:2::1 ffff:ffff::
 router eigrp4 1 enable
 router eigrp6 1 enable
 no shutdown
 no log-link-change
 exit
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
router eigrp4 1
 vrf v1
 router-id 4.4.4.3
 as 1
 redistribute connected
 exit
!
router eigrp6 1
 vrf v1
 router-id 6.6.6.3
 as 1
 redistribute connected
 exit
!
interface loopback1
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
 ipv4 address 1.1.1.6 255.255.255.252
 ipv6 address 1234:2::2 ffff:ffff::
 router eigrp4 1 enable
 router eigrp6 1 enable
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.9 255.255.255.252
 ipv6 address 1234:3::1 ffff:ffff::
 router eigrp4 1 enable
 router eigrp6 1 enable
 no shutdown
 no log-link-change
 exit
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
router eigrp4 1
 vrf v1
 router-id 4.4.4.4
 as 1
 redistribute connected
 exit
!
router eigrp6 1
 vrf v1
 router-id 6.6.6.4
 as 1
 redistribute connected
 exit
!
interface loopback1
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
 ipv4 address 1.1.1.10 255.255.255.252
 ipv6 address 1234:3::2 ffff:ffff::
 router eigrp4 1 enable
 router eigrp6 1 enable
 no shutdown
 no log-link-change
 exit
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
!
!
!
end
```

## **Verification**

```
r2#
r2#
r2#show ipv4 eigrp 1 sum
r2#show ipv4 eigrp 1 sum
 |~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | iface     | peer    | learned | adverted | uptime   |
 |-----------|---------|---------|----------|----------|
 | ethernet1 | 1.1.1.1 | 1       | 5        | 00:00:04 |
 | ethernet2 | 1.1.1.6 | 3       | 3        | 00:00:04 |
 |___________|_________|_________|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 eigrp 1 sum
r2#show ipv6 eigrp 1 sum
 |~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | iface     | peer      | learned | adverted | uptime   |
 |-----------|-----------|---------|----------|----------|
 | ethernet1 | 1234:1::1 | 1       | 5        | 00:00:04 |
 | ethernet2 | 1234:2::2 | 3       | 3        | 00:00:04 |
 |___________|___________|_________|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 eigrp 1 rou
r2#show ipv4 eigrp 1 rou
 |~~~~~~|~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
 | typ  | prefix     | metric | iface     | hop     | time     |
 |------|------------|--------|-----------|---------|----------|
 | C    | 1.1.1.0/30 | 0/0    | ethernet1 | null    | 00:00:05 |
 | C    | 1.1.1.4/30 | 0/0    | ethernet2 | null    | 00:00:05 |
 | null | 1.1.1.8/30 | 90/10  | ethernet2 | 1.1.1.6 | 00:00:05 |
 | null | 2.2.2.1/32 | 90/10  | ethernet1 | 1.1.1.1 | 00:00:05 |
 | C    | 2.2.2.2/32 | 0/0    | loopback1 | null    | 00:00:05 |
 | null | 2.2.2.3/32 | 90/10  | ethernet2 | 1.1.1.6 | 00:00:05 |
 | null | 2.2.2.4/32 | 90/20  | ethernet2 | 1.1.1.6 | 00:00:05 |
 |______|____________|________|___________|_________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 eigrp 1 rou
r2#show ipv6 eigrp 1 rou
 |~~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
 | typ  | prefix      | metric | iface     | hop       | time     |
 |------|-------------|--------|-----------|-----------|----------|
 | C    | 1234:1::/32 | 0/0    | ethernet1 | null      | 00:00:05 |
 | C    | 1234:2::/32 | 0/0    | ethernet2 | null      | 00:00:05 |
 | null | 1234:3::/32 | 90/10  | ethernet2 | 1234:2::2 | 00:00:05 |
 | null | 4321::1/128 | 90/10  | ethernet1 | 1234:1::1 | 00:00:05 |
 | C    | 4321::2/128 | 0/0    | loopback1 | null      | 00:00:05 |
 | null | 4321::3/128 | 90/10  | ethernet2 | 1234:2::2 | 00:00:05 |
 | null | 4321::4/128 | 90/20  | ethernet2 | 1234:2::2 | 00:00:05 |
 |______|_____________|________|___________|___________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 route v1
r2#show ipv4 route v1
 |~~~~~|~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix     | metric | iface     | hop     | time     |
 |-----|------------|--------|-----------|---------|----------|
 | C   | 1.1.1.0/30 | 0/0    | ethernet1 | null    | 00:00:05 |
 | LOC | 1.1.1.2/32 | 0/1    | ethernet1 | null    | 00:00:05 |
 | C   | 1.1.1.4/30 | 0/0    | ethernet2 | null    | 00:00:05 |
 | LOC | 1.1.1.5/32 | 0/1    | ethernet2 | null    | 00:00:05 |
 | D   | 1.1.1.8/30 | 90/10  | ethernet2 | 1.1.1.6 | 00:00:05 |
 | D   | 2.2.2.1/32 | 90/10  | ethernet1 | 1.1.1.1 | 00:00:05 |
 | C   | 2.2.2.2/32 | 0/0    | loopback1 | null    | 00:00:05 |
 | D   | 2.2.2.3/32 | 90/10  | ethernet2 | 1.1.1.6 | 00:00:05 |
 | D   | 2.2.2.4/32 | 90/20  | ethernet2 | 1.1.1.6 | 00:00:05 |
 |_____|____________|________|___________|_________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 route v1
r2#show ipv6 route v1
 |~~~~~|~~~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix        | metric | iface     | hop       | time     |
 |-----|---------------|--------|-----------|-----------|----------|
 | C   | 1234:1::/32   | 0/0    | ethernet1 | null      | 00:00:05 |
 | LOC | 1234:1::2/128 | 0/1    | ethernet1 | null      | 00:00:05 |
 | C   | 1234:2::/32   | 0/0    | ethernet2 | null      | 00:00:05 |
 | LOC | 1234:2::1/128 | 0/1    | ethernet2 | null      | 00:00:05 |
 | D   | 1234:3::/32   | 90/10  | ethernet2 | 1234:2::2 | 00:00:05 |
 | D   | 4321::1/128   | 90/10  | ethernet1 | 1234:1::1 | 00:00:05 |
 | C   | 4321::2/128   | 0/0    | loopback1 | null      | 00:00:05 |
 | D   | 4321::3/128   | 90/10  | ethernet2 | 1234:2::2 | 00:00:05 |
 | D   | 4321::4/128   | 90/20  | ethernet2 | 1234:2::2 | 00:00:05 |
 |_____|_______________|________|___________|___________|__________|
r2#
r2#
```
