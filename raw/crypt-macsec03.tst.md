# Example: macsec over ethernet bridge

## **Topology diagram**

![topology](/img/crypt-macsec03.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
crypto ipsec ips
 group 02
 cipher aes256
 hash sha1
 key $v10$dGVzdGVy
 exit
!
bridge 1
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface bvi1
 no description
 macsec ips
 vrf forwarding v1
 ipv4 address 1.1.1.1 255.255.255.0
 ipv6 address 1234::1 ffff::
 no shutdown
 no log-link-change
 exit
!
interface ethernet1
 no description
 bridge-group 1
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
crypto ipsec ips
 group 02
 cipher aes256
 hash sha1
 key $v10$dGVzdGVy
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface ethernet1
 no description
 macsec ips
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.0
 ipv6 address 1234::2 ffff::
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
