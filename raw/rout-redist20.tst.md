# Example: redistribution change in metric

## **Topology diagram**

![topology](/img/rout-redist20.tst.png)

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
 sequence 10 set metric set 1000
 !
 exit
!
vrf definition v1
 rd 1:1
 exit
!
router isis4 1
 vrf v1
 net-id 22.4444.0000.1111.00
 traffeng-id ::
 is-type both
 redistribute connected route-map rm1
 exit
!
router isis6 1
 vrf v1
 net-id 22.6666.0000.1111.00
 traffeng-id ::
 is-type both
 redistribute connected route-map rm1
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
 no shutdown
 no log-link-change
 exit
!
interface ethernet1.11
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.252
 router isis4 1 enable
 router isis4 1 circuit both
 no shutdown
 no log-link-change
 exit
!
interface ethernet1.12
 no description
 vrf forwarding v1
 ipv6 address 1234:1::1 ffff:ffff::
 router isis6 1 enable
 router isis6 1 circuit both
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
router isis4 1
 vrf v1
 net-id 22.4444.0000.2222.00
 traffeng-id ::
 is-type both
 redistribute connected
 redistribute isis4 2
 exit
!
router isis4 2
 vrf v1
 net-id 22.4444.0000.2222.00
 traffeng-id ::
 is-type both
 redistribute connected
 redistribute isis4 1
 exit
!
router isis6 1
 vrf v1
 net-id 22.6666.0000.2222.00
 traffeng-id ::
 is-type both
 redistribute connected
 redistribute isis6 2
 exit
!
router isis6 2
 vrf v1
 net-id 22.6666.0000.2222.00
 traffeng-id ::
 is-type both
 redistribute connected
 redistribute isis6 1
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
 no shutdown
 no log-link-change
 exit
!
interface ethernet1.11
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.252
 router isis4 1 enable
 router isis4 1 circuit both
 no shutdown
 no log-link-change
 exit
!
interface ethernet1.12
 no description
 vrf forwarding v1
 ipv6 address 1234:1::2 ffff:ffff::
 router isis6 1 enable
 router isis6 1 circuit both
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 no shutdown
 no log-link-change
 exit
!
interface ethernet2.11
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.5 255.255.255.252
 router isis4 2 enable
 router isis4 2 circuit both
 no shutdown
 no log-link-change
 exit
!
interface ethernet2.12
 no description
 vrf forwarding v1
 ipv6 address 1234:2::1 ffff:ffff::
 router isis6 2 enable
 router isis6 2 circuit both
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
 sequence 10 action deny
 sequence 10 match metric 2000-4000
 !
 sequence 20 action permit
 !
 exit
!
vrf definition v1
 rd 1:1
 exit
!
router isis4 1
 vrf v1
 net-id 22.4444.0000.3333.00
 traffeng-id ::
 is-type both
 level2 route-map-from rm1
 level1 route-map-from rm1
 redistribute connected
 exit
!
router isis6 1
 vrf v1
 net-id 22.6666.0000.3333.00
 traffeng-id ::
 is-type both
 level2 route-map-from rm1
 level1 route-map-from rm1
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
 no shutdown
 no log-link-change
 exit
!
interface ethernet1.11
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.6 255.255.255.252
 router isis4 1 enable
 router isis4 1 circuit both
 no shutdown
 no log-link-change
 exit
!
interface ethernet1.12
 no description
 vrf forwarding v1
 ipv6 address 1234:2::2 ffff:ffff::
 router isis6 1 enable
 router isis6 1 circuit both
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
