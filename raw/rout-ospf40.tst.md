# Example: ospf autoroute

## **Topology diagram**

![topology](/img/rout-ospf40.tst.png)

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
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.1
 traffeng-id ::
 area 0 enable
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
interface loopback1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.11 255.255.255.255
 ipv6 address 4321::11 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
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
 router ospf4 1 enable
 router ospf4 1 area 0
 router ospf6 1 enable
 router ospf6 1 area 0
 no shutdown
 no log-link-change
 exit
!
interface serial2
 no description
 encapsulation hdlc
 vrf forwarding v1
 ipv4 address 9.9.8.1 255.255.255.0
 ipv4 autoroute ospf4 1 2.2.2.2 9.9.8.2
 ipv6 address 9998::1 ffff::
 ipv6 autoroute ospf6 1 4321::2 9998::2
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
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.2
 traffeng-id ::
 area 0 enable
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
interface loopback1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.12 255.255.255.255
 ipv6 address 4321::12 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
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
 router ospf4 1 enable
 router ospf4 1 area 0
 router ospf6 1 enable
 router ospf6 1 area 0
 no shutdown
 no log-link-change
 exit
!
interface serial2
 no description
 encapsulation hdlc
 vrf forwarding v1
 ipv4 address 9.9.8.2 255.255.255.0
 ipv4 autoroute ospf4 1 2.2.2.1 9.9.8.1
 ipv6 address 9998::2 ffff::
 ipv6 autoroute ospf6 1 4321::1 9998::1
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
 | serial1   | 9.9.9.1 | 4.4.4.1  | 00:00:41 |
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
 | serial1   | 9999::1 | 6.6.6.1  | 00:00:41 |
 |___________|_________|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 ospf 1 dat 0
r2#show ipv4 ospf 1 dat 0
 |~~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~~~|~~~~~|~~~~~~~~~~|
 | routerid | lsaid    | sequence | type       | len | time     |
 |----------|----------|----------|------------|-----|----------|
 | 4.4.4.1  | 4.4.4.1  | 80000004 | router     | 28  | 00:00:40 |
 | 4.4.4.2  | 4.4.4.2  | 80000004 | router     | 28  | 00:00:38 |
 | 4.4.4.1  | 0.0.0.0  | 80000002 | asExternal | 16  | 01:00:42 |
 | 4.4.4.2  | 0.0.0.0  | 80000002 | asExternal | 16  | 01:00:42 |
 | 4.4.4.1  | 2.2.2.1  | 80000001 | asExternal | 16  | 00:00:42 |
 | 4.4.4.2  | 2.2.2.2  | 80000001 | asExternal | 16  | 00:00:42 |
 | 4.4.4.1  | 2.2.2.11 | 80000001 | asExternal | 16  | 00:00:42 |
 | 4.4.4.2  | 2.2.2.12 | 80000001 | asExternal | 16  | 00:00:42 |
 | 4.4.4.1  | 9.9.8.0  | 80000001 | asExternal | 16  | 00:00:41 |
 | 4.4.4.2  | 9.9.8.0  | 80000001 | asExternal | 16  | 00:00:42 |
 | 4.4.4.1  | 9.9.9.0  | 80000001 | asExternal | 16  | 00:00:41 |
 | 4.4.4.2  | 9.9.9.0  | 80000001 | asExternal | 16  | 00:00:42 |
 |__________|__________|__________|____________|_____|__________|
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
 | 6.6.6.1  | 10013 | 80000001 | link       | 24  | 00:00:42 |
 | 6.6.6.2  | 10013 | 80000001 | link       | 24  | 00:00:42 |
 | 6.6.6.1  | 0     | 80000003 | router     | 20  | 00:00:40 |
 | 6.6.6.2  | 0     | 80000003 | router     | 20  | 00:00:38 |
 | 6.6.6.1  | 10013 | 80000001 | prefix     | 20  | 00:00:41 |
 | 6.6.6.2  | 10013 | 80000001 | prefix     | 20  | 00:00:42 |
 | 6.6.6.1  | 0     | 80000001 | asExternal | 28  | 00:00:42 |
 | 6.6.6.2  | 0     | 80000002 | asExternal | 28  | 00:00:42 |
 | 6.6.6.1  | 1     | 80000001 | asExternal | 28  | 00:00:42 |
 | 6.6.6.2  | 1     | 80000001 | asExternal | 28  | 00:00:42 |
 | 6.6.6.1  | 2     | 80000001 | asExternal | 16  | 00:00:41 |
 | 6.6.6.2  | 2     | 80000001 | asExternal | 16  | 00:00:42 |
 | 6.6.6.1  | 3     | 80000001 | asExternal | 16  | 00:00:41 |
 | 6.6.6.2  | 3     | 80000001 | asExternal | 16  | 00:00:42 |
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
 |~~~~~|~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix      | metric | iface     | hop     | time     |
 |-----|-------------|--------|-----------|---------|----------|
 | O   | 2.2.2.1/32  | 110/0  | serial1   | 9.9.9.1 | 00:00:39 |
 | C   | 2.2.2.2/32  | 0/0    | loopback0 | null    | 00:00:39 |
 | O   | 2.2.2.11/32 | 110/0  | serial2   | 9.9.8.1 | 00:00:39 |
 | C   | 2.2.2.12/32 | 0/0    | loopback1 | null    | 00:00:39 |
 | C   | 9.9.8.0/24  | 0/0    | serial2   | null    | 00:00:39 |
 | LOC | 9.9.8.2/32  | 0/1    | serial2   | null    | 00:00:39 |
 | C   | 9.9.9.0/24  | 0/0    | serial1   | null    | 00:00:39 |
 | LOC | 9.9.9.2/32  | 0/1    | serial1   | null    | 00:00:39 |
 |_____|_____________|________|___________|_________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 route v1
r2#show ipv6 route v1
 |~~~~~|~~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix       | metric | iface     | hop     | time     |
 |-----|--------------|--------|-----------|---------|----------|
 | O   | 4321::1/128  | 110/0  | serial1   | 9999::1 | 00:00:39 |
 | C   | 4321::2/128  | 0/0    | loopback0 | null    | 00:00:39 |
 | O   | 4321::11/128 | 110/0  | serial2   | 9998::1 | 00:00:39 |
 | C   | 4321::12/128 | 0/0    | loopback1 | null    | 00:00:39 |
 | C   | 9998::/16    | 0/0    | serial2   | null    | 00:00:39 |
 | LOC | 9998::2/128  | 0/1    | serial2   | null    | 00:00:39 |
 | C   | 9999::/16    | 0/0    | serial1   | null    | 00:00:39 |
 | LOC | 9999::2/128  | 0/1    | serial1   | null    | 00:00:39 |
 |_____|______________|________|___________|_________|__________|
r2#
r2#
```
