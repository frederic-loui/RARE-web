# Example: pvrp tag

## **Topology diagram**

![topology](/img/rout-pvrp17.tst.png)

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
 sequence 10 set tag set 1234
 !
 exit
!
vrf definition v1
 rd 1:1
 exit
!
router pvrp4 1
 vrf v1
 router-id 4.4.4.1
 redistribute connected route-map rm1
 exit
!
router pvrp6 1
 vrf v1
 router-id 6.6.6.1
 redistribute connected route-map rm1
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
 router pvrp4 1 enable
 router pvrp6 1 enable
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
route-map rm1
 sequence 10 action deny
 sequence 10 match tag 1234
 !
 sequence 20 action permit
 !
 exit
!
vrf definition v1
 rd 1:1
 exit
!
router pvrp4 1
 vrf v1
 router-id 4.4.4.2
 redistribute connected
 exit
!
router pvrp6 1
 vrf v1
 router-id 6.6.6.2
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
 router pvrp4 1 enable
 router pvrp4 1 route-map-out rm1
 router pvrp6 1 enable
 router pvrp6 1 route-map-out rm1
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.5 255.255.255.252
 ipv6 address 1234:2::1 ffff:ffff::
 router pvrp4 1 enable
 router pvrp4 1 route-map-out rm1
 router pvrp6 1 enable
 router pvrp6 1 route-map-out rm1
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
 sequence 10 set tag set 1234
 !
 exit
!
vrf definition v1
 rd 1:1
 exit
!
router pvrp4 1
 vrf v1
 router-id 4.4.4.3
 redistribute connected route-map rm1
 exit
!
router pvrp6 1
 vrf v1
 router-id 6.6.6.3
 redistribute connected route-map rm1
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
 router pvrp4 1 enable
 router pvrp6 1 enable
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