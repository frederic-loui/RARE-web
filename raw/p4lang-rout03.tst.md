# Example: p4lang: mpls

## **Topology diagram**

![topology](/img/p4lang-rout03.tst.png)

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
vrf definition v9
 rd 1:1
 exit
!
interface loopback0
 no description
 vrf forwarding v1
 ipv4 address 2.2.2.101 255.255.255.255
 ipv6 address 4321::101 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface loopback9
 no description
 vrf forwarding v9
 ipv4 address 10.10.10.227 255.255.255.255
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v9
 ipv4 address 10.11.12.254 255.255.255.0
 no shutdown
 no log-link-change
 exit
!
interface ethernet2
 no description
 no shutdown
 no log-link-change
 exit
!
interface sdn1
 no description
 mtu 1500
 macaddr 000e.5205.5438
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.0
 ipv6 address 1234:1::1 ffff:ffff::
 ipv6 enable
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
interface sdn2
 no description
 mtu 1500
 macaddr 002c.7340.2a75
 vrf forwarding v1
 ipv4 address 1.1.2.1 255.255.255.0
 ipv6 address 1234:2::1 ffff:ffff::
 ipv6 enable
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
interface sdn3
 no description
 mtu 1500
 macaddr 000d.427d.7b4a
 vrf forwarding v1
 ipv4 address 1.1.3.1 255.255.255.0
 ipv6 address 1234:3::1 ffff:ffff::
 ipv6 enable
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
interface sdn4
 no description
 mtu 1500
 macaddr 0014.430d.2062
 vrf forwarding v1
 ipv4 address 1.1.4.1 255.255.255.0
 ipv6 address 1234:4::1 ffff:ffff::
 ipv6 enable
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 2.2.2.103 255.255.255.255 1.1.1.2
ipv4 route v1 2.2.2.104 255.255.255.255 1.1.2.2
ipv4 route v1 2.2.2.105 255.255.255.255 1.1.3.2
ipv4 route v1 2.2.2.106 255.255.255.255 1.1.4.2
!
ipv6 route v1 4321::103 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::2
ipv6 route v1 4321::104 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:2::2
ipv6 route v1 4321::105 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:3::2
ipv6 route v1 4321::106 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:4::2
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
server p4lang p4
 export-vrf v1 1
 export-port sdn1 1
 export-port sdn2 2
 export-port sdn3 3
 export-port sdn4 4
 interconnect ethernet2
 vrf v9
 exit
!
server dhcp4 eth1
 pool 10.11.12.1 10.11.12.99
 gateway 10.11.12.254
 netmask 255.255.255.0
 dns-server 10.10.10.227
 domain-name p4l
 static 0000.0000.2222 10.11.12.111
 interface ethernet1
 vrf v9
 exit
!
!
end
```

**r2:**
```
hostname r2
```

**r3:**
```
hostname r3
buggy
!
logging file debug ../binTmp/zzz-log-r3.run
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
 ipv4 address 2.2.2.103 255.255.255.255
 ipv6 address 4321::103 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.0
 ipv4 access-group-in test4
 ipv6 address 1234:1::2 ffff:ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 1.1.2.0 255.255.255.0 1.1.1.1
ipv4 route v1 1.1.3.0 255.255.255.0 1.1.1.1
ipv4 route v1 1.1.4.0 255.255.255.0 1.1.1.1
ipv4 route v1 2.2.2.101 255.255.255.255 1.1.1.1
ipv4 route v1 2.2.2.104 255.255.255.255 1.1.1.1
ipv4 route v1 2.2.2.105 255.255.255.255 1.1.1.1
ipv4 route v1 2.2.2.106 255.255.255.255 1.1.1.1
!
ipv6 route v1 1234:2:: ffff:ffff:: 1234:1::1
ipv6 route v1 1234:3:: ffff:ffff:: 1234:1::1
ipv6 route v1 1234:4:: ffff:ffff:: 1234:1::1
ipv6 route v1 4321::101 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::1
ipv6 route v1 4321::104 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::1
ipv6 route v1 4321::105 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::1
ipv6 route v1 4321::106 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:1::1
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

**r4:**
```
hostname r4
buggy
!
logging file debug ../binTmp/zzz-log-r4.run
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
 ipv4 address 2.2.2.104 255.255.255.255
 ipv6 address 4321::104 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.2.2 255.255.255.0
 ipv4 access-group-in test4
 ipv6 address 1234:2::2 ffff:ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 1.1.1.0 255.255.255.0 1.1.2.1
ipv4 route v1 1.1.3.0 255.255.255.0 1.1.2.1
ipv4 route v1 1.1.4.0 255.255.255.0 1.1.2.1
ipv4 route v1 2.2.2.101 255.255.255.255 1.1.2.1
ipv4 route v1 2.2.2.103 255.255.255.255 1.1.2.1
ipv4 route v1 2.2.2.105 255.255.255.255 1.1.2.1
ipv4 route v1 2.2.2.106 255.255.255.255 1.1.2.1
!
ipv6 route v1 1234:1:: ffff:ffff:: 1234:2::1
ipv6 route v1 1234:3:: ffff:ffff:: 1234:2::1
ipv6 route v1 1234:4:: ffff:ffff:: 1234:2::1
ipv6 route v1 4321::101 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:2::1
ipv6 route v1 4321::103 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:2::1
ipv6 route v1 4321::105 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:2::1
ipv6 route v1 4321::106 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:2::1
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

**r5:**
```
hostname r5
buggy
!
logging file debug ../binTmp/zzz-log-r5.run
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
 ipv4 address 2.2.2.105 255.255.255.255
 ipv6 address 4321::105 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.3.2 255.255.255.0
 ipv4 access-group-in test4
 ipv6 address 1234:3::2 ffff:ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 1.1.1.0 255.255.255.0 1.1.3.1
ipv4 route v1 1.1.2.0 255.255.255.0 1.1.3.1
ipv4 route v1 1.1.4.0 255.255.255.0 1.1.3.1
ipv4 route v1 2.2.2.101 255.255.255.255 1.1.3.1
ipv4 route v1 2.2.2.103 255.255.255.255 1.1.3.1
ipv4 route v1 2.2.2.104 255.255.255.255 1.1.3.1
ipv4 route v1 2.2.2.106 255.255.255.255 1.1.3.1
!
ipv6 route v1 1234:1:: ffff:ffff:: 1234:3::1
ipv6 route v1 1234:2:: ffff:ffff:: 1234:3::1
ipv6 route v1 1234:4:: ffff:ffff:: 1234:3::1
ipv6 route v1 4321::101 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:3::1
ipv6 route v1 4321::103 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:3::1
ipv6 route v1 4321::104 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:3::1
ipv6 route v1 4321::106 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:3::1
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

**r6:**
```
hostname r6
buggy
!
logging file debug ../binTmp/zzz-log-r6.run
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
 ipv4 address 2.2.2.106 255.255.255.255
 ipv6 address 4321::106 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.4.2 255.255.255.0
 ipv4 access-group-in test4
 ipv6 address 1234:4::2 ffff:ffff::
 ipv6 access-group-in test6
 mpls enable
 mpls ldp4
 mpls ldp6
 no shutdown
 no log-link-change
 exit
!
!
ipv4 route v1 1.1.1.0 255.255.255.0 1.1.4.1
ipv4 route v1 1.1.2.0 255.255.255.0 1.1.4.1
ipv4 route v1 1.1.3.0 255.255.255.0 1.1.4.1
ipv4 route v1 2.2.2.101 255.255.255.255 1.1.4.1
ipv4 route v1 2.2.2.103 255.255.255.255 1.1.4.1
ipv4 route v1 2.2.2.104 255.255.255.255 1.1.4.1
ipv4 route v1 2.2.2.105 255.255.255.255 1.1.4.1
!
ipv6 route v1 1234:1:: ffff:ffff:: 1234:4::1
ipv6 route v1 1234:2:: ffff:ffff:: 1234:4::1
ipv6 route v1 1234:3:: ffff:ffff:: 1234:4::1
ipv6 route v1 4321::101 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:4::1
ipv6 route v1 4321::103 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:4::1
ipv6 route v1 4321::104 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:4::1
ipv6 route v1 4321::105 ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff 1234:4::1
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
