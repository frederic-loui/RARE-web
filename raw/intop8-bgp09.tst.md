# Example: interop8: bgp addpath

## **Topology diagram**

![topology](/img/intop8-bgp09.tst.png)

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
 ipv6 address 1234::1 ffff::
 no shutdown
 no log-link-change
 exit
!
router bgp4 1
 vrf v1
 local-as 1
 router-id 4.4.4.1
 address-family unicast
 neighbor 1.1.1.2 remote-as 1
 no neighbor 1.1.1.2 description
 neighbor 1.1.1.2 local-as 1
 neighbor 1.1.1.2 address-family unicast
 neighbor 1.1.1.2 distance 200
 neighbor 1.1.1.2 additional-path-rx unicast
 neighbor 1.1.1.2 additional-path-tx unicast
 redistribute connected
 exit
!
router bgp6 1
 vrf v1
 local-as 1
 router-id 6.6.6.1
 address-family unicast
 neighbor 1234::2 remote-as 1
 no neighbor 1234::2 description
 neighbor 1234::2 local-as 1
 neighbor 1234::2 address-family unicast
 neighbor 1234::2 distance 200
 neighbor 1234::2 additional-path-rx unicast
 neighbor 1234::2 additional-path-tx unicast
 redistribute connected
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
ip forwarding
ipv6 forwarding
interface lo
 ip addr 2.2.2.2/32
 ipv6 addr 4321::2/128
 exit
interface ens3
 ip address 1.1.1.2/24
 ipv6 address 1234::2/64
 no shutdown
 exit
router bgp 1
 neighbor 1.1.1.1 remote-as 1
 neighbor 1234::1 remote-as 1
 address-family ipv4 unicast
  neighbor 1.1.1.1 activate
  no neighbor 1234::1 activate
  neighbor 1.1.1.1 addpath-tx-all
  redistribute connected
 address-family ipv6 unicast
  no neighbor 1.1.1.1 activate
  neighbor 1234::1 activate
  neighbor 1234::1 addpath-tx-all
  redistribute connected
 exit
```
