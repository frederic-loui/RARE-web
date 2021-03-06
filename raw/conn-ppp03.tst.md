# Example: ppp with radius authentication

## **Topology diagram**

![topology](/img/conn-ppp03.tst.png)

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
interface serial1
 no description
 encapsulation ppp
 ppp username c
 ppp password $v10$Yw==
 ppp ip4cp close
 ppp ip6cp close
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.0
 ipv6 address 1234::1 ffff::
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
aaa radius usr
 no log-error
 secret $v10$Yw==
 server 2.2.2.2
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.1 255.255.255.0
 no shutdown
 no log-link-change
 exit
!
interface serial1
 no description
 encapsulation ppp
 ppp authentication usr
 ppp ip4cp close
 ppp ip6cp close
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
client proxy p1
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
aaa userlist usr
 no log-error
 username c
 username c password $v10$Yw==
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.2 255.255.255.0
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
server radius rad
 authentication usr
 secret $v10$Yw==
 logging
 vrf v1
 exit
!
!
end
```
