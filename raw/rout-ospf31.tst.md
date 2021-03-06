# Example: ospf prefix withdraw

## **Topology diagram**

![topology](/img/rout-ospf31.tst.png)

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
router ospf4 1
 vrf v1
 router-id 4.4.4.1
 traffeng-id 0.0.0.0
 area 0 enable
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.1
 traffeng-id ::
 area 0 enable
 exit
!
interface loopback1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.1 255.255.255.255
 ipv6 address 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 router ospf4 1 enable
 router ospf4 1 area 0
 router ospf6 1 enable
 router ospf6 1 area 0
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.252
 ipv6 address 1234:1::1 ffff:ffff::
 router ospf4 1 enable
 router ospf4 1 area 0
 router ospf6 1 enable
 router ospf6 1 area 0
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
router ospf4 1
 vrf v1
 router-id 4.4.4.2
 traffeng-id 0.0.0.0
 area 0 enable
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.2
 traffeng-id ::
 area 0 enable
 exit
!
interface loopback1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.2 255.255.255.255
 ipv6 address 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 router ospf4 1 enable
 router ospf4 1 area 0
 router ospf6 1 enable
 router ospf6 1 area 0
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.252
 ipv6 address 1234:1::2 ffff:ffff::
 router ospf4 1 enable
 router ospf4 1 area 0
 router ospf6 1 enable
 router ospf6 1 area 0
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
r2#show ipv4 ospf 1 nei
r2#show ipv4 ospf 1 nei
 |~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | interface | address | routerid | uptime   |
 |-----------|---------|----------|----------|
 | ethernet1 | 1.1.1.1 | 4.4.4.1  | 00:00:10 |
 |___________|_________|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 ospf 1 nei
r2#show ipv6 ospf 1 nei
 |~~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | interface | address   | routerid | uptime   |
 |-----------|-----------|----------|----------|
 | ethernet1 | 1234:1::1 | 6.6.6.1  | 00:00:10 |
 |___________|___________|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 ospf 1 dat 0
r2#show ipv4 ospf 1 dat 0
 |~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~|~~~~~|~~~~~~~~~~|
 | routerid | lsaid   | sequence | type   | len | time     |
 |----------|---------|----------|--------|-----|----------|
 | 4.4.4.1  | 4.4.4.1 | 80000006 | router | 40  | 00:00:03 |
 | 4.4.4.2  | 4.4.4.2 | 80000004 | router | 40  | 00:00:07 |
 |__________|_________|__________|________|_____|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 ospf 1 dat 0
r2#show ipv6 ospf 1 dat 0
 |~~~~~~~~~~|~~~~~~~|~~~~~~~~~~|~~~~~~~~|~~~~~|~~~~~~~~~~|
 | routerid | lsaid | sequence | type   | len | time     |
 |----------|-------|----------|--------|-----|----------|
 | 6.6.6.1  | 10011 | 80000003 | link   | 24  | 00:00:03 |
 | 6.6.6.2  | 10011 | 80000001 | link   | 24  | 00:00:11 |
 | 6.6.6.1  | 10012 | 80000001 | link   | 24  | 00:00:10 |
 | 6.6.6.2  | 10012 | 80000001 | link   | 24  | 00:00:11 |
 | 6.6.6.1  | 0     | 80000002 | router | 20  | 00:00:10 |
 | 6.6.6.2  | 0     | 80000002 | router | 20  | 00:00:07 |
 | 6.6.6.1  | 10011 | 80000003 | prefix | 32  | 00:00:03 |
 | 6.6.6.2  | 10011 | 80000001 | prefix | 32  | 00:00:11 |
 | 6.6.6.1  | 10012 | 80000001 | prefix | 20  | 00:00:10 |
 | 6.6.6.2  | 10012 | 80000001 | prefix | 20  | 00:00:11 |
 |__________|_______|__________|________|_____|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 ospf 1 tre 0
r2#show ipv4 ospf 1 tre 0
`--4.4.4.2
   `--4.4.4.1
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 ospf 1 tre 0
r2#show ipv6 ospf 1 tre 0
`--6.6.6.2/00000000
   `--6.6.6.1/00000000
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
 | C   | 1.1.1.0/30 | 0/0    | ethernet1 | null    | 00:00:02 |
 | LOC | 1.1.1.2/32 | 0/1    | ethernet1 | null    | 00:00:02 |
 | O   | 2.2.2.1/32 | 110/1  | ethernet1 | 1.1.1.1 | 00:00:02 |
 | C   | 2.2.2.2/32 | 0/0    | loopback1 | null    | 00:00:02 |
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
 | C   | 1234:1::/32   | 0/0    | ethernet1 | null      | 00:00:02 |
 | LOC | 1234:1::2/128 | 0/1    | ethernet1 | null      | 00:00:02 |
 | O   | 4321::1/128   | 110/1  | ethernet1 | 1234:1::1 | 00:00:02 |
 | C   | 4321::2/128   | 0/0    | loopback1 | null      | 00:00:02 |
 |_____|_______________|________|___________|___________|__________|
r2#
r2#
```
