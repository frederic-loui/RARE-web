# Example: lapb over l2tp3

## **Topology diagram**

![topology](/img/conn-l2tp05.tst.png)

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
interface dialer1
 no description
 encapsulation lapb
 lapb mode dce
 vrf forwarding v1
 ipv4 address 2.2.2.1 255.255.255.252
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
proxy-profile p1
 vrf v1
 exit
!
vpdn l2tp
 interface dialer1
 proxy p1
 target 1.1.1.2
 vcid 1234
 pwtype hdlc
 protocol l2tp3
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
interface dialer1
 no description
 encapsulation lapb
 lapb mode dte
 vrf forwarding v1
 ipv4 address 2.2.2.2 255.255.255.252
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.0
 ipv6 address 1234::2 ffff::
 no shutdown
 no log-link-change
 exit
!
proxy-profile p1
 vrf v1
 exit
!
vpdn l2tp
 interface dialer1
 proxy p1
 target 1.1.1.1
 vcid 1234
 direction incoming
 pwtype hdlc
 protocol l2tp3
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
