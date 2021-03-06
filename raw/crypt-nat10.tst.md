# Example: nat64 translation

## **Topology diagram**

![topology](/img/crypt-nat10.tst.png)

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
interface loopback1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.2 255.255.255.255
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.252
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 0.0.0.0 0.0.0.0 1.1.1.2
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
access-list nat
 sequence 10 deny all fe80:: ffff:: all any all
 sequence 20 deny all any all fe80:: ffff:: all
 sequence 30 deny all any all ff00:: ff00:: all
 sequence 40 deny all 6464:: ffff:ffff:ffff:ffff:: all 6464:: ffff:ffff:ffff:ffff:: all
 sequence 50 permit all any all 6464:: ffff:ffff:ffff:ffff:: all
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.252
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 vrf forwarding v1
 ipv6 address 1234::1 ffff:ffff::
 no shutdown
 no log-link-change
 exit
!
interface tunnel1
 no description
 tunnel key 96
 tunnel vrf v1
 tunnel source ethernet2
 tunnel destination 6464::a01:4042
 tunnel mode 6to4
 vrf forwarding v1
 ipv4 address 10.1.64.65 255.255.255.252
 ipv6 address 6464::a01:4042 ffff:ffff:ffff:ffff:ffff:ffff::
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 0.0.0.0 0.0.0.0 1.1.1.1
!
ipv6 route v1 :: :: 1234::2
!
!
!
!
ipv6 nat v1 sequence 10 srclist nat interface tunnel1
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
vrf definition v1
 rd 1:1
 exit
!
interface loopback1
 no description
 vrf forwarding v1
 ipv6 address 8888::8 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv6 address 1234::2 ffff:ffff::
 no shutdown
 no log-link-change
 exit
!
!
!
ipv6 route v1 :: :: 1234::1
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
