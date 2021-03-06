# Example: ldp over te

## **Topology diagram**

![topology](/img/mpls-te10.tst.png)

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
 sequence 10 deny all 4321:: ffff:: all 4321:: ffff:: all
 sequence 20 permit all any all any all
 exit
!
vrf definition v1
 rd 1:1
 label-mode per-prefix
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
 ipv4 address 1.1.1.1 255.255.255.0
 ipv4 access-group-in test4
 ipv6 address 1234::1 ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls rsvp4
 mpls rsvp6
 no shutdown
 no log-link-change
 exit
!
interface tunnel1
 no description
 tunnel vrf v1
 tunnel source ethernet1
 tunnel destination 1.1.1.2
 tunnel mode p2pte
 vrf forwarding v1
 ipv4 address 1.1.2.1 255.255.255.252
 ipv4 access-group-in test4
 mpls enable
 mpls ldp4
 no shutdown
 no log-link-change
 exit
!
interface tunnel2
 no description
 tunnel vrf v1
 tunnel source ethernet1
 tunnel destination 1234::2
 tunnel mode p2pte
 vrf forwarding v1
 ipv6 address 2345::1 ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 2.2.2.2 255.255.255.255 1.1.2.2
!
ipv6 route v1 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 2345::2
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
 sequence 10 deny all 4321:: ffff:: all 4321:: ffff:: all
 sequence 20 permit all any all any all
 exit
!
vrf definition v1
 rd 1:1
 label-mode per-prefix
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
 ipv4 address 1.1.1.2 255.255.255.0
 ipv4 access-group-in test4
 ipv6 address 1234::2 ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls rsvp4
 mpls rsvp6
 no shutdown
 no log-link-change
 exit
!
interface tunnel1
 no description
 tunnel vrf v1
 tunnel source ethernet1
 tunnel destination 1.1.1.1
 tunnel mode p2pte
 vrf forwarding v1
 ipv4 address 1.1.2.2 255.255.255.252
 ipv4 access-group-in test4
 mpls enable
 mpls ldp4
 no shutdown
 no log-link-change
 exit
!
interface tunnel2
 no description
 tunnel vrf v1
 tunnel source ethernet1
 tunnel destination 1234::1
 tunnel mode p2pte
 vrf forwarding v1
 ipv6 address 2345::2 ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 2.2.2.1 255.255.255.255 1.1.2.1
!
ipv6 route v1 4321::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 2345::1
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
