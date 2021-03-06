# Example: ospf prefix movement

## **Topology diagram**

![topology](/img/rout-ospf37.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
route-map rm1
 sequence 10 action permit
 sequence 10 set metric set 10
 !
 exit
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
 advertise 2.2.2.1/32 route-map rm1
 advertise 2.2.2.222/32 route-map rm1
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.1
 traffeng-id ::
 area 0 enable
 advertise 4321::1/128 route-map rm1
 advertise 4321::222/128 route-map rm1
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
interface loopback2
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.222 255.255.255.255
 ipv6 address 4321::222 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface loopback3
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.101 255.255.255.255
 ipv6 address 4321::101 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
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
server telnet tel
 port 666
 no exec authorization
 no login authentication
 vrf v1
 exit
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
 advertise 2.2.2.2/32
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.2
 traffeng-id ::
 area 0 enable
 advertise 4321::2/128
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
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.5 255.255.255.252
 ipv6 address 1234:2::1 ffff:ffff::
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

**r3:**
```
hostname r3
buggy
!
logging file debug ../binTmp/zzz-log-r3.run
!
route-map rm1
 sequence 10 action permit
 sequence 10 set metric set 20
 !
 exit
!
vrf definition v1
 rd 1:1
 exit
!
router ospf4 1
 vrf v1
 router-id 4.4.4.3
 traffeng-id 0.0.0.0
 area 0 enable
 advertise 2.2.2.3/32 route-map rm1
 advertise 2.2.2.222/32 route-map rm1
 exit
!
router ospf6 1
 vrf v1
 router-id 6.6.6.3
 traffeng-id ::
 area 0 enable
 advertise 4321::3/128 route-map rm1
 advertise 4321::222/128 route-map rm1
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
interface loopback2
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.222 255.255.255.255
 ipv6 address 4321::222 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface loopback3
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.103 255.255.255.255
 ipv6 address 4321::103 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.6 255.255.255.252
 ipv6 address 1234:2::2 ffff:ffff::
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
server telnet tel
 port 666
 no exec authorization
 no login authentication
 vrf v1
 exit
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
 | ethernet1 | 1.1.1.1 | 4.4.4.1  | 00:00:30 |
 | ethernet2 | 1.1.1.6 | 4.4.4.3  | 00:00:30 |
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
 | ethernet1 | 1234:1::1 | 6.6.6.1  | 00:00:30 |
 | ethernet2 | 1234:2::2 | 6.6.6.3  | 00:00:30 |
 |___________|___________|__________|__________|
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 ospf 1 dat 0
r2#show ipv4 ospf 1 dat 0
 |~~~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~~|~~~~~~~~~~~~|~~~~~|~~~~~~~~~~|
 | routerid | lsaid     | sequence | type       | len | time     |
 |----------|-----------|----------|------------|-----|----------|
 | 4.4.4.1  | 4.4.4.1   | 80000004 | router     | 28  | 00:00:30 |
 | 4.4.4.2  | 4.4.4.2   | 80000007 | router     | 64  | 00:00:30 |
 | 4.4.4.3  | 4.4.4.3   | 80000004 | router     | 28  | 00:00:30 |
 | 4.4.4.1  | 2.2.2.1   | 80000003 | asExternal | 16  | 00:00:08 |
 | 4.4.4.2  | 2.2.2.2   | 80000002 | asExternal | 16  | 00:00:31 |
 | 4.4.4.3  | 2.2.2.3   | 80000001 | asExternal | 16  | 00:00:31 |
 | 4.4.4.1  | 2.2.2.222 | 80000003 | asExternal | 16  | 00:00:08 |
 | 4.4.4.3  | 2.2.2.222 | 80000001 | asExternal | 16  | 00:00:31 |
 |__________|___________|__________|____________|_____|__________|
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
 | 6.6.6.2  | 10011 | 80000001 | link       | 24  | 00:00:31 |
 | 6.6.6.2  | 10012 | 80000001 | link       | 24  | 00:00:31 |
 | 6.6.6.2  | 10013 | 80000001 | link       | 24  | 00:00:31 |
 | 6.6.6.1  | 10014 | 80000001 | link       | 24  | 00:00:31 |
 | 6.6.6.3  | 10014 | 80000001 | link       | 24  | 00:00:30 |
 | 6.6.6.1  | 0     | 80000003 | router     | 20  | 00:00:30 |
 | 6.6.6.2  | 0     | 80000004 | router     | 36  | 00:00:30 |
 | 6.6.6.3  | 0     | 80000003 | router     | 20  | 00:00:30 |
 | 6.6.6.2  | 10011 | 80000001 | prefix     | 32  | 00:00:31 |
 | 6.6.6.2  | 10012 | 80000001 | prefix     | 20  | 00:00:31 |
 | 6.6.6.2  | 10013 | 80000001 | prefix     | 20  | 00:00:31 |
 | 6.6.6.1  | 10014 | 80000001 | prefix     | 20  | 00:00:31 |
 | 6.6.6.3  | 10014 | 80000001 | prefix     | 20  | 00:00:30 |
 | 6.6.6.1  | 0     | 80000003 | asExternal | 28  | 00:00:08 |
 | 6.6.6.2  | 0     | 80000001 | asExternal | 28  | 00:00:31 |
 | 6.6.6.3  | 0     | 80000001 | asExternal | 28  | 00:00:31 |
 | 6.6.6.1  | 1     | 80000003 | asExternal | 28  | 00:00:08 |
 | 6.6.6.3  | 1     | 80000001 | asExternal | 28  | 00:00:31 |
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
  |`--4.4.4.1
   `--4.4.4.3
r2#
r2#
```

```
r2#
r2#
r2#show ipv6 ospf 1 tre 0
r2#show ipv6 ospf 1 tre 0
`--6.6.6.2/00000000
  |`--6.6.6.1/00000000
   `--6.6.6.3/00000000
r2#
r2#
```

```
r2#
r2#
r2#show ipv4 route v1
r2#show ipv4 route v1
 |~~~~~|~~~~~~~~~~~~~~|~~~~~~~~|~~~~~~~~~~~|~~~~~~~~~|~~~~~~~~~~|
 | typ | prefix       | metric | iface     | hop     | time     |
 |-----|--------------|--------|-----------|---------|----------|
 | C   | 1.1.1.0/30   | 0/0    | ethernet1 | null    | 00:00:07 |
 | LOC | 1.1.1.2/32   | 0/1    | ethernet1 | null    | 00:00:07 |
 | C   | 1.1.1.4/30   | 0/0    | ethernet2 | null    | 00:00:07 |
 | LOC | 1.1.1.5/32   | 0/1    | ethernet2 | null    | 00:00:07 |
 | O   | 2.2.2.1/32   | 110/10 | ethernet1 | 1.1.1.1 | 00:00:07 |
 | C   | 2.2.2.2/32   | 0/0    | loopback1 | null    | 00:00:07 |
 | O   | 2.2.2.3/32   | 110/20 | ethernet2 | 1.1.1.6 | 00:00:07 |
 | O   | 2.2.2.222/32 | 110/10 | ethernet1 | 1.1.1.1 | 00:00:07 |
 |_____|______________|________|___________|_________|__________|
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
 | C   | 1234:1::/32   | 0/0    | ethernet1 | null      | 00:00:07 |
 | LOC | 1234:1::2/128 | 0/1    | ethernet1 | null      | 00:00:07 |
 | C   | 1234:2::/32   | 0/0    | ethernet2 | null      | 00:00:07 |
 | LOC | 1234:2::1/128 | 0/1    | ethernet2 | null      | 00:00:07 |
 | O   | 4321::1/128   | 110/10 | ethernet1 | 1234:1::1 | 00:00:07 |
 | C   | 4321::2/128   | 0/0    | loopback1 | null      | 00:00:07 |
 | O   | 4321::3/128   | 110/20 | ethernet2 | 1234:2::2 | 00:00:07 |
 | O   | 4321::222/128 | 110/10 | ethernet1 | 1234:1::1 | 00:00:07 |
 |_____|_______________|________|___________|___________|__________|
r2#
r2#
```
