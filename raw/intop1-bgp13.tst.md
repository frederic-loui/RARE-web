# Example: interop1: bgp vpnv6

## **Topology diagram**

![topology](/img/intop1-bgp13.tst.png)

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
 label-mode per-prefix
 exit
!
vrf definition v2
 rd 1:2
 rt-import 1:2
 rt-export 1:2
 exit
!
vrf definition v3
 rd 1:3
 rt-import 1:3
 rt-export 1:3
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
interface loopback2
 no description
 vrf forwarding v2
 ipv4 address 9.9.2.1 255.255.255.255
 ipv6 address 9992::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface loopback3
 no description
 vrf forwarding v3
 ipv4 address 9.9.3.1 255.255.255.255
 ipv6 address 9993::1 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.0
 ipv6 address 1234::1 ffff::
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
router bgp4 1
 vrf v1
 local-as 1
 router-id 4.4.4.1
 address-family ovpnuni
 neighbor 2.2.2.2 remote-as 1
 no neighbor 2.2.2.2 description
 neighbor 2.2.2.2 local-as 1
 neighbor 2.2.2.2 address-family ovpnuni
 neighbor 2.2.2.2 distance 200
 neighbor 2.2.2.2 update-source loopback0
 neighbor 2.2.2.2 send-community standard extended
 afi-ovrf v2 enable
 afi-ovrf v2 redistribute connected
 afi-ovrf v3 enable
 afi-ovrf v3 redistribute connected
 exit
!
router bgp6 1
 vrf v1
 local-as 1
 router-id 6.6.6.1
 address-family ovpnuni
 neighbor 4321::2 remote-as 1
 no neighbor 4321::2 description
 neighbor 4321::2 local-as 1
 neighbor 4321::2 address-family ovpnuni
 neighbor 4321::2 distance 200
 neighbor 4321::2 update-source loopback0
 neighbor 4321::2 send-community standard extended
 afi-ovrf v2 enable
 afi-ovrf v2 redistribute connected
 afi-ovrf v3 enable
 afi-ovrf v3 redistribute connected
 exit
!
!
ipv4 route v1 2.2.2.2 255.255.255.255 1.1.1.2
!
ipv6 route v1 4321::2 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234::2
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
ip routing
ipv6 unicast-routing
mpls ldp explicit-null
vrf definition v2
 rd 1:2
 route-target export 1:2
 route-target import 1:2
 address-family ipv4
 address-family ipv6
 exit
vrf definition v3
 rd 1:3
 route-target export 1:3
 route-target import 1:3
 address-family ipv4
 address-family ipv6
 exit
interface loopback0
 ip addr 2.2.2.2 255.255.255.255
 ipv6 addr 4321::2/128
 exit
interface loopback2
 vrf forwarding v2
 ip address 9.9.2.2 255.255.255.255
 ipv6 address 9992::2/128
 exit
interface loopback3
 vrf forwarding v3
 ip address 9.9.3.2 255.255.255.255
 ipv6 address 9993::2/128
 exit
interface gigabit1
 ip address 1.1.1.2 255.255.255.0
 ipv6 address 1234::2/64
 mpls ip
 no shutdown
 exit
ip route 2.2.2.1 255.255.255.255 1.1.1.1
ipv6 route 4321::1/128 1234::1
router bgp 1
 neighbor 2.2.2.1 remote-as 1
 neighbor 2.2.2.1 update-source loopback0
 neighbor 4321::1 remote-as 1
 neighbor 4321::1 update-source loopback0
 neighbor 4321::1 shutdown
 address-family vpnv4 unicast
  bgp scan-time 5
  neighbor 4321::1 activate
  neighbor 4321::1 send-community both
 address-family vpnv6 unicast
  bgp scan-time 5
  neighbor 2.2.2.1 activate
  neighbor 2.2.2.1 send-community both
 address-family ipv4 vrf v2
  redistribute connected
 address-family ipv6 vrf v2
  redistribute connected
 address-family ipv4 vrf v3
  redistribute connected
 address-family ipv6 vrf v3
  redistribute connected
 exit
```
