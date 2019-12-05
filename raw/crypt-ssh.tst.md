# Example: ssh test

## **Topology diagram**

![topology](/img/crypt-ssh.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
crypto rsakey myrsa import MIICXQIBAAKBgQC0nrND79O5ol/LSSysGPYtULCE2h38ddL7JsXDUM3CCIt5REfaumcXwFHcMW2brgH452tw+qY0mewHpxNBr043S/ZBKPwZHdYhSHhtiTD+L5HTmZIGo3U+u11Ty5me6puog7ETsLgQdZA3UMjNVTIq4qvMRIMRTXg9YhFn0vs1rwIDAQABAoGAK23VUMqDsCj4u5p2oVLHLpIuP2Nqvl9eQYFLH/F35+XCE4B1foQ/cZiOllFUN5CZbM3IKbw65n70H8rueGa8eWr1y/sU0NSAZpiv7nYvf/UMrBhKnoX0aC7FIj5LFI4yUxHqgsh6aKiTI77FDcxbsepVxHh86K1tV1Iz6PAIJDECQQD14A7oRUiHIoQHWBHmOUACF6bL3z8+dXzbYJ0KiQcPyPSdLsf2EueFfFmV6+sDMZDh56i4BgOIHNgJ5xPAcSn1AkEAvA69j9FcmvhyHt8xp9/kUEw++6mBbl+8Gtca1krvGyRM9rJOii5aTw2nsrob55nkRyXT19ig0lpxBCetllbmkwJAZgnsZdyK8voSDPIxy3eq5rNmF2m413TusiRwR/UCJzzgt3K4rZi+2Jj4nM1hpNunIvl+/ZARWyWNSt7JRa4xGQJBAIeadXQz+nvAtMGwHWU427BPGodrten53HDaNP7a78l5honJDzsyq2ofpZIKAz8gx+vJyhT1nxmvn5joZ5JlvUMCQQDLsvHanVDX7TS02u2+ULkqf66s/1ePPDWnW1Zjh0R3Wjn8dHaDcZLnibwWZ8PFUD8E5NXevW/GWwqKBG5JenqX
!
crypto dsakey mydsa import MIIBuwIBAAKBgQCcggj1iLzr28C2Y85FdyVSBHlU698Bzks6Iy24MGJiPxI+eSB2NoQ+S7afv+QFugskXJBawo3+gXXu5hfXJc8tVOwfOvTX2jW9nNrP5NP6Za5mNKZVENNGASlrEQtqAawHE6A7w6/J2FCXvwjYO7SvB46ky5Cvs7ox9TkTkTGJZwIVANDg+p6L15yjsWI8i0fYQnGuSslhAoGAQj7BDUJpeIJ33WPX9wJJ6NtGZ29vzmXe6cCOGEqQs7xSr847qcGzEXtUGSWtgZ6t1iI6RtESu+qmcg59foemTeyrCa4ux6Ydn7YARfFOVL4PEtAlFvkvEi/U7LRl9HKQB+sPJW4hU/fqRaTlSjE39pGWf3d6OGHLN6aMSFn+w/UCgYBGoyEsesSc8RWMy7+cY0SeFymWzLb30wqzG0AvrHQhE1xyWY7xOm9wJ7YE+n9KxjQeIR7PJMrmclW/P/XpZHkCgFvAr3aAFK/w8Fcku24CF4gW5w1W2O7V2ThDsrn6f+JGfEGRBvfxYin9KwX9N35IAQpQisMCtQT9rneWMkiaAgIVAM7N+Iwu4dULRYiYvGjmYW8eBf+d
!
crypto ecdsakey myecdsa import MEQCAQEEEACtT1ZwRkfDhndc4zXN/1GgBwYFK4EEAByhJAMiAASucERsdb81h9FyhNEdSrMbnK55VVz3W22q6dz+ggcGQA==
!
aaa userlist usr
 no log-error
 username c
 username c password $v10$Yw==
 username c privilege 14
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface loopback0
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
server telnet ssh
 security protocol ssh
 security authentication usr
 security rsakey myrsa
 security dsakey mydsa
 security ecdsakey myecdsa
 port 666
 no exec authorization
 no login authentication
 vrf v1
 exit
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
interface ethernet1
 no description
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.0
 ipv6 address 1234::2 ffff::
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
