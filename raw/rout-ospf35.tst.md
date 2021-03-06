# Example: ospf auto mesh tunnel

## **Topology diagram**

![topology](/img/rout-ospf35.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
access-list test4
 sequence 10 deny 1 any all any all
 sequence 20 permit all any all any all
 exit
!
access-list test6
 sequence 10 deny 58 any all any all
 sequence 20 permit all any all any all
 exit
!
prefix-list all
 sequence 10 permit 0.0.0.0/0 ge 0 le 32
 sequence 20 permit ::/0 ge 0 le 128
 exit
!
vrf definition v1
 rd 1:1
 label-mode per-prefix
 exit
!
router ospf4 1
 vrf v1
 router-id 4.4.4.1
 traffeng-id 0.0.0.0
 area 0 enable
 redistribute connected
 automesh all
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.1
 traffeng-id ::
 area 0 enable
 redistribute connected
 automesh all
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
interface serial1
 no description
 encapsulation hdlc
 vrf forwarding v1
 ipv4 address 9.9.9.1 255.255.255.0
 ipv4 access-group-in test4
 ipv6 address 9999::1 ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls rsvp4
 mpls rsvp6
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
access-list test4
 sequence 10 deny 1 any all any all
 sequence 20 permit all any all any all
 exit
!
access-list test6
 sequence 10 deny 58 any all any all
 sequence 20 permit all any all any all
 exit
!
prefix-list all
 sequence 10 permit 0.0.0.0/0 ge 0 le 32
 sequence 20 permit ::/0 ge 0 le 128
 exit
!
vrf definition v1
 rd 1:1
 label-mode per-prefix
 exit
!
router ospf4 1
 vrf v1
 router-id 4.4.4.2
 traffeng-id 0.0.0.0
 area 0 enable
 redistribute connected
 automesh all
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.2
 traffeng-id ::
 area 0 enable
 redistribute connected
 automesh all
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
interface serial1
 no description
 encapsulation hdlc
 vrf forwarding v1
 ipv4 address 9.9.9.2 255.255.255.0
 ipv4 access-group-in test4
 ipv6 address 9999::2 ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls rsvp4
 mpls rsvp6
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
 | serial1   | 9.9.9.1 | 4.4.4.1  | 00:00:23 |
 |___________|_________|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 ospf 1 nei
r2#show ipv6 ospf 1 nei
 |~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|
 | interface | address | routerid | uptime   |
 |-----------|---------|----------|----------|
 | serial1   | 9999::1 | 6.6.6.1  | 00:00:23 |
 |___________|_________|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 ospf 1 dat 0
r2#show ipv4 ospf 1 dat 0
 |~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~~~|~~~~~|~~~~~~~~~~|
 | routerid | lsaid   | sequence | type       | len | time     |
 |----------|---------|----------|------------|-----|----------|
 | 4.4.4.1  | 4.4.4.1 | 80000004 | router     | 28  | 00:00:23 |
 | 4.4.4.2  | 4.4.4.2 | 80000004 | router     | 28  | 00:00:23 |
 | 4.4.4.1  | 0.0.0.0 | 80000002 | asExternal | 16  | 01:00:34 |
 | 4.4.4.1  | 2.2.2.1 | 80000001 | asExternal | 16  | 00:00:34 |
 | 4.4.4.2  | 2.2.2.2 | 80000001 | asExternal | 16  | 00:00:34 |
 | 4.4.4.1  | 9.9.9.0 | 80000001 | asExternal | 16  | 00:00:28 |
 | 4.4.4.2  | 9.9.9.0 | 80000001 | asExternal | 16  | 00:00:29 |
 |__________|_________|__________|____________|_____|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 ospf 1 dat 0
r2#show ipv6 ospf 1 dat 0
 |~~~~~~~~~~|~~~~~~~|~~~~~~~~~~|~~~~~~~~~~~~|~~~~~|~~~~~~~~~~|
 | routerid | lsaid | sequence | type       | len | time     |
 |----------|-------|----------|------------|-----|----------|
 | 6.6.6.1  | 10012 | 80000001 | link       | 24  | 00:00:34 |
 | 6.6.6.2  | 10012 | 80000001 | link       | 24  | 00:00:34 |
 | 6.6.6.1  | 0     | 80000003 | router     | 20  | 00:00:23 |
 | 6.6.6.2  | 0     | 80000003 | router     | 20  | 00:00:23 |
 | 6.6.6.1  | 10012 | 80000001 | prefix     | 20  | 00:00:28 |
 | 6.6.6.2  | 10012 | 80000001 | prefix     | 20  | 00:00:29 |
 | 6.6.6.1  | 0     | 80000001 | asExternal | 28  | 00:00:34 |
 | 6.6.6.2  | 0     | 80000001 | asExternal | 28  | 00:00:34 |
 | 6.6.6.1  | 1     | 80000001 | asExternal | 16  | 00:00:28 |
 | 6.6.6.2  | 1     | 80000001 | asExternal | 16  | 00:00:29 |
 |__________|_______|__________|____________|_____|__________|
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
 | O   | 2.2.2.1/32 | 110/0  | serial1   | 9.9.9.1 | 00:00:24 |
 | C   | 2.2.2.2/32 | 0/0    | loopback0 | null    | 00:00:24 |
 | C   | 9.9.9.0/24 | 0/0    | serial1   | null    | 00:00:24 |
 | MSH | 9.9.9.1/32 | 0/3    | serial1   | 9.9.9.1 | never    |
 | LOC | 9.9.9.2/32 | 0/1    | serial1   | null    | 00:00:24 |
 |_____|____________|________|___________|_________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 route v1
r2#show ipv6 route v1
 |~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix      | metric | iface     | hop     | time     |
 |-----|-------------|--------|-----------|---------|----------|
 | O   | 4321::1/128 | 110/0  | serial1   | 9999::1 | 00:00:24 |
 | C   | 4321::2/128 | 0/0    | loopback0 | null    | 00:00:24 |
 | C   | 9999::/16   | 0/0    | serial1   | null    | 00:00:24 |
 | MSH | 9999::1/128 | 0/3    | serial1   | 9999::1 | never    |
 | LOC | 9999::2/128 | 0/1    | serial1   | null    | 00:00:24 |
 |_____|_____________|________|___________|_________|__________|
r2#
r2#
```
