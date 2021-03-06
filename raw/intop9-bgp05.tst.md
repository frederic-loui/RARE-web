# Example: interop9: bgp metric

## **Topology diagram**

![topology](/img/intop9-bgp05.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
route-map rm1
 sequence 10 action deny
 sequence 10 match metric 1234
 !
 sequence 20 action permit
 !
 exit
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
 neighbor 1.1.1.2 route-map-in rm1
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
 neighbor 1234::2 route-map-in rm1
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
set interfaces ge-0/0/0.0 family inet address 1.1.1.2/24
set interfaces ge-0/0/0.0 family inet6 address 1234::2/64
set interfaces lo0.0 family inet address 2.2.2.2/32
set interfaces lo0.0 family inet6 address 4321::2/128
set interfaces lo0.0 family inet address 2.2.2.3/32
set interfaces lo0.0 family inet6 address 4321::3/128
set interfaces lo0.0 family inet address 2.2.2.4/32
set interfaces lo0.0 family inet6 address 4321::4/128
set routing-options autonomous-system 1
set policy-options policy-statement ps1 term 1 from interface [ 2.2.2.3 4321::3 ]
set policy-options policy-statement ps1 term 1 then metric 1234
set policy-options policy-statement ps1 term 1 then accept
set policy-options policy-statement ps1 term 2 from protocol direct
set policy-options policy-statement ps1 term 2 then metric 4321
set policy-options policy-statement ps1 term 2 then accept
set protocols bgp export ps1
set protocols bgp group peers type internal
set protocols bgp group peers peer-as 1
set protocols bgp group peers neighbor 1.1.1.1
set protocols bgp group peers neighbor 1234::1
commit
```
