# Example: isis multi-topology with te

## **Topology diagram**

![topology](/img/rout-isis38.tst.png)

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
router isis4 1
 vrf v1
 net-id 48.4444.0000.1111.00
 traffeng-id 4.4.4.1
 is-type both
 multi-topology
 level2 traffeng
 level1 traffeng
 redistribute connected
 exit
!
router isis6 1
 vrf v1
 net-id 48.6666.0000.1111.00
 traffeng-id 6.6.6.1
 is-type both
 multi-topology
 level2 traffeng
 level1 traffeng
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
 net-id 48.4444.0000.2222.00
 traffeng-id 4.4.4.2
 is-type both
 multi-topology
 level2 traffeng
 level1 traffeng
 redistribute connected
 exit
!
router isis6 1
 vrf v1
 net-id 48.6666.0000.2222.00
 traffeng-id 6.6.6.1
 is-type both
 multi-topology
 level2 traffeng
 level1 traffeng
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
