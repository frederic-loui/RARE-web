# Example: interop2: rip prefix withdraw

## **Topology diagram**

![topology](/img/intop2-rip02.tst.png)

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
router rip4 1
 vrf v1
 redistribute connected
 exit
!
router rip6 1
 vrf v1
 redistribute connected
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
 ipv6 address fe80::1 ffff::
 router rip4 1 enable
 router rip4 1 update-timer 5000
 router rip4 1 hold-time 15000
 router rip4 1 flush-time 15000
 router rip6 1 enable
 router rip6 1 update-timer 5000
 router rip6 1 hold-time 15000
 router rip6 1 flush-time 15000
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
interface gigabit0/0/0/0
 ipv4 address 1.1.1.2 255.255.255.0
 ipv6 address fe80::2 link-local
 no shutdown
 exit
interface loopback0
 ipv4 addr 2.2.2.2 255.255.255.255
 ipv6 addr 4321::2/128
 exit
route-policy a
 set rip-metric 5
 pass
end-policy
router rip
 timers basic 5 15 15 16
 redistribute connected route-policy a
 interface gigabit0/0/0/0
root
commit
```
