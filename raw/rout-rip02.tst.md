# Example: rip in chain

## **Topology diagram**

![topology](/img/rout-rip02.tst.png)

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
router rip4 1
 vrf v1
 redistribute connected
 exit
!
router rip6 1
 vrf v1
 redistribute connected
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
 ipv4 address 1.1.1.1 255.255.255.252
 ipv6 address 1234:1::1 ffff:ffff::
 router rip4 1 enable
 router rip6 1 enable
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
router rip4 1
 vrf v1
 redistribute connected
 exit
!
router rip6 1
 vrf v1
 redistribute connected
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
 ipv4 address 1.1.1.2 255.255.255.252
 ipv6 address 1234:1::2 ffff:ffff::
 router rip4 1 enable
 router rip6 1 enable
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.5 255.255.255.252
 ipv6 address 1234:2::1 ffff:ffff::
 router rip4 1 enable
 router rip6 1 enable
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
router rip4 1
 vrf v1
 redistribute connected
 exit
!
router rip6 1
 vrf v1
 redistribute connected
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
 ipv4 address 1.1.1.6 255.255.255.252
 ipv6 address 1234:2::2 ffff:ffff::
 router rip4 1 enable
 router rip6 1 enable
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.9 255.255.255.252
 ipv6 address 1234:3::1 ffff:ffff::
 router rip4 1 enable
 router rip6 1 enable
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
router rip4 1
 vrf v1
 redistribute connected
 exit
!
router rip6 1
 vrf v1
 redistribute connected
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
 ipv4 address 1.1.1.10 255.255.255.252
 ipv6 address 1234:3::2 ffff:ffff::
 router rip4 1 enable
 router rip6 1 enable
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
r2#show ipv4 rip 1 sum
r2#show ipv4 rip 1 sum
 |~~~~~~~~~~~|~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | interface | learn | neighbor | uptime   |
 |-----------|-------|----------|----------|
 | ethernet1 | 2     | 1.1.1.1  | 00:01:07 |
 | ethernet2 | 4     | 1.1.1.6  | 00:01:07 |
 |___________|_______|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 rip 1 sum
r2#show ipv6 rip 1 sum
 |~~~~~~~~~~~|~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
 | interface | learn | neighbor  | uptime   |
 |-----------|-------|-----------|----------|
 | ethernet1 | 2     | 1234:1::1 | 00:01:37 |
 | ethernet2 | 4     | 1234:2::2 | 00:01:37 |
 |___________|_______|___________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 rip 1 dat
r2#show ipv4 rip 1 dat
 |~~~~~|~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix     | metric | iface     | hop     | time     |
 |-----|------------|--------|-----------|---------|----------|
 | R   | 1.1.1.0/30 | 1/0    | ethernet1 | null    | 00:00:37 |
 | R   | 1.1.1.4/30 | 1/0    | ethernet2 | null    | 00:00:37 |
 | R   | 1.1.1.8/30 | 120/1  | ethernet2 | 1.1.1.6 | 00:00:37 |
 | R   | 2.2.2.1/32 | 120/1  | ethernet1 | 1.1.1.1 | 00:01:07 |
 | R   | 2.2.2.3/32 | 120/1  | ethernet2 | 1.1.1.6 | 00:00:37 |
 | R   | 2.2.2.4/32 | 120/2  | ethernet2 | 1.1.1.6 | 00:00:37 |
 |_____|____________|________|___________|_________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 rip 1 dat
r2#show ipv6 rip 1 dat
 |~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix      | metric | iface     | hop       | time     |
 |-----|-------------|--------|-----------|-----------|----------|
 | R   | 1234:1::/32 | 1/0    | ethernet1 | null      | 00:01:07 |
 | R   | 1234:2::/32 | 1/0    | ethernet2 | null      | 00:01:07 |
 | R   | 1234:3::/32 | 120/1  | ethernet2 | 1234:2::2 | 00:01:07 |
 | R   | 4321::1/128 | 120/1  | ethernet1 | 1234:1::1 | 00:01:37 |
 | R   | 4321::3/128 | 120/1  | ethernet2 | 1234:2::2 | 00:01:07 |
 | R   | 4321::4/128 | 120/2  | ethernet2 | 1234:2::2 | 00:01:07 |
 |_____|_____________|________|___________|___________|__________|
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
 | C   | 1.1.1.0/30 | 0/0    | ethernet1 | null    | 00:00:38 |
 | LOC | 1.1.1.2/32 | 0/1    | ethernet1 | null    | 00:00:38 |
 | C   | 1.1.1.4/30 | 0/0    | ethernet2 | null    | 00:00:38 |
 | LOC | 1.1.1.5/32 | 0/1    | ethernet2 | null    | 00:00:38 |
 | R   | 1.1.1.8/30 | 120/1  | ethernet2 | 1.1.1.6 | 00:00:38 |
 | R   | 2.2.2.1/32 | 120/1  | ethernet1 | 1.1.1.1 | 00:01:07 |
 | C   | 2.2.2.2/32 | 0/0    | loopback0 | null    | 00:00:38 |
 | R   | 2.2.2.3/32 | 120/1  | ethernet2 | 1.1.1.6 | 00:00:38 |
 | R   | 2.2.2.4/32 | 120/2  | ethernet2 | 1.1.1.6 | 00:00:38 |
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
 | C   | 1234:1::/32   | 0/0    | ethernet1 | null      | 00:01:08 |
 | LOC | 1234:1::2/128 | 0/1    | ethernet1 | null      | 00:01:08 |
 | C   | 1234:2::/32   | 0/0    | ethernet2 | null      | 00:01:08 |
 | LOC | 1234:2::1/128 | 0/1    | ethernet2 | null      | 00:01:08 |
 | R   | 1234:3::/32   | 120/1  | ethernet2 | 1234:2::2 | 00:01:08 |
 | R   | 4321::1/128   | 120/1  | ethernet1 | 1234:1::1 | 00:01:38 |
 | C   | 4321::2/128   | 0/0    | loopback0 | null      | 00:01:08 |
 | R   | 4321::3/128   | 120/1  | ethernet2 | 1234:2::2 | 00:01:08 |
 | R   | 4321::4/128   | 120/2  | ethernet2 | 1234:2::2 | 00:01:08 |
 |_____|_______________|________|___________|___________|__________|
r2#
r2#
```
