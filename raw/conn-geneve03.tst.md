# Example: geneve over loopback

## **Topology diagram**

![topology](/img/conn-geneve03.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
bridge 1
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface loopback0
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.101 255.255.255.255
 no shutdown
 no log-link-change
 exit
!
interface bvi1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.1 255.255.255.0
 ipv6 address 4321::1 ffff::
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
proxy-profile p1
 vrf v1
 source loopback0
 exit
!
vpdn vx
 bridge-group 1
 proxy p1
 target 1.1.1.102
 vcid 123
 protocol geneve
 exit
!
!
ipv4 route v1 1.1.1.102 255.255.255.255 1.1.1.2
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
bridge 1
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface loopback0
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.102 255.255.255.255
 no shutdown
 no log-link-change
 exit
!
interface bvi1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.2 255.255.255.0
 ipv6 address 4321::2 ffff::
 no shutdown
 no log-link-change
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
proxy-profile p1
 vrf v1
 source loopback0
 exit
!
vpdn vx
 bridge-group 1
 proxy p1
 target 1.1.1.101
 vcid 123
 protocol geneve
 exit
!
!
ipv4 route v1 1.1.1.101 255.255.255.255 1.1.1.1
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
