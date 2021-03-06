# Example: eigrp with bfd

## **Topology diagram**

![topology](/img/rout-eigrp14.tst.png)

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
 ipv4 bfd 100 100 3
 ipv6 address 1234:1::1 ffff:ffff::
 ipv6 bfd 100 100 3
 router eigrp4 1 enable
 router eigrp4 1 bfd
 router eigrp6 1 enable
 router eigrp6 1 bfd
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.5 255.255.255.252
 ipv4 bfd 100 100 3
 ipv6 address 1234:2::1 ffff:ffff::
 ipv6 bfd 100 100 3
 router eigrp4 1 enable
 router eigrp4 1 bfd
 router eigrp4 1 delay-in 100
 router eigrp6 1 enable
 router eigrp6 1 bfd
 router eigrp6 1 delay-in 100
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
 ipv4 bfd 100 100 3
 ipv6 address 1234:1::2 ffff:ffff::
 ipv6 bfd 100 100 3
 router eigrp4 1 enable
 router eigrp4 1 bfd
 router eigrp6 1 enable
 router eigrp6 1 bfd
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.6 255.255.255.252
 ipv4 bfd 100 100 3
 ipv6 address 1234:2::2 ffff:ffff::
 ipv6 bfd 100 100 3
 router eigrp4 1 enable
 router eigrp4 1 bfd
 router eigrp4 1 delay-in 100
 router eigrp6 1 enable
 router eigrp6 1 bfd
 router eigrp6 1 delay-in 100
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
