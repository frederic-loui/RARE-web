# Example: ppp with packet over tls

## **Topology diagram**

![topology](/img/conn-pckou19.tst.png)

## **Configuration**

**r1:**
```
hostname r1
buggy
!
logging file debug ../binTmp/zzz-log-r1.run
!
crypto rsakey r import MIIBOwIBAAJBAL3hOLQfweWW8Vc0CevyNxjB/Ab0Zht4gLJ6G6VVHTkS13tVpikIq5ejkNf3dPXEKvCRfKEfAMxshEAn9EbGO6sCAwEAAQJADaSyM1cY0ote+foWQGreBJxjpIhJEe49qRVXCTEtcV1Gg3LDSS2ywDrZ6rZfgkCwsxwGl5f4wKQeRKadl4w6SQIhAOUKwRoi6qcA0Ni5QAXhtEcAXDBbNp0wj0IK8eH0RhonAiEA1Dp5G4xJ1tGOz4UtSHInwj5YG2uo+1waNKEhSpSCGN0CIQCYkVRV26JhAjsb0jJieJlEnmnKUEgN5xXWUUIUWEoGCwIgKU7IS8hehuV6+oFdcwBlJLWlyJCH74TZxOyn4jBNsjECIQDApNxeuIfbkSVWEQtB1y2mvOe7neLVdyx0mmbcRNMe9Q==
!
crypto dsakey d import MIH6AgEAAkEA0S/bT8d8AXRIlY0xa7xkt2UKAwscYxlxqeymNpE2B5zC/XiXBn0O+v5vX+6lr7O2HSZPsCBr7m4g6CjxTGhHsQIVAOsrY/eQH3Tg7dNiOSRgPddLJQ1hAkEAwU7GE1yyJsDazWwve/FdLx7OpmQteeiIaJis/QO1w/IrWY5fSrk/t7rvcvPSuvb6ruTlzNKie96wbLhr/PlNuwJBAMXSqGisRHeC8h/w2VrPwcvzrJ/rJqolux2/4cnnAM8wKM5HeMawCEsv+Emci091J+AwEG+4Sc+Tu6GSkO/H39MCFQDAUjwjqS3sPZjKde/I2uZLs3LFHA==
!
crypto ecdsakey ed import MEQCAQEEEACtT1ZwRkfDhndc4zXN/1GgBwYFK4EEAByhJAMiAASucERsdb81h9FyhNEdSrMbnK55VVz3W22q6dz+ggcGQA==
!
crypto certificate d import dsa d MIIBijCCAUmgAwIBAgIEXLxJpzAJBgcqhkjOOAQDMA0xCzAJBgNVBAMTAnIxMB4XDTEyMDkwMjEzMDI1OVoXDTIyMDgzMTEzMDI1OVowDTELMAkGA1UEAxMCcjEwgfIwgakGByqGSM44BAEwgZ0CQQDRL9tPx3wBdEiVjTFrvGS3ZQoDCxxjGXGp7KY2kTYHnML9eJcGfQ76/m9f7qWvs7YdJk+wIGvubiDoKPFMaEexAhUA6ytj95AfdODt02I5JGA910slDWECQQDBTsYTXLImwNrNbC978V0vHs6mZC156IhomKz9A7XD8itZjl9KuT+3uu9y89K69vqu5OXM0qJ73rBsuGv8+U27A0QAAkEAxdKoaKxEd4LyH/DZWs/By/Osn+smqiW7Hb/hyecAzzAozkd4xrAISy/4SZyLT3Un4DAQb7hJz5O7oZKQ78ff0zAJBgcqhkjOOAQDAzAAMC0CFQCKPXwWufyQseH8aprFK/YlnOlHiwIUHb70eiFivtAWfMsXokzH3MCwr8E=
!
crypto certificate ed import ecdsa ed MIHlMIGOoAMCAQICBHdcELYwCQYHKoZIzj0EATAOMQwwCgYDVQQDEwNydHIwHhcNMTQxMDIzMDc1NjQ4WhcNMjQxMDIwMDc1NjQ4WjAOMQwwCgYDVQQDEwNydHIwNjAQBgcqhkjOPQIBBgUrgQQAHAMiAASucERsdb81h9FyhNEdSrMbnK55VVz3W22q6dz+ggcGQDAJBgcqhkjOPQQBA0cAMEQCEQCj1kFVhYyMmgeEsIMMCTalAi8Wvrl1ZghtS9ybZuiheuKZCFHKHPDOWPd4C6dKxyvvBsLep0GvqeRn/Un7+8QB0w==
!
crypto certificate r import rsa r MIIBBjCBtKADAgECAgRMTNRlMAsGCSqGSIb3DQEBBTANMQswCQYDVQQDEwJyMTAeFw0xMjA5MDIxMzAyNTNaFw0yMjA4MzExMzAyNTNaMA0xCzAJBgNVBAMTAnIxMFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBAL3hOLQfweWW8Vc0CevyNxjB/Ab0Zht4gLJ6G6VVHTkS13tVpikIq5ejkNf3dPXEKvCRfKEfAMxshEAn9EbGO6sCAwEAATALBgkqhkiG9w0BAQUDQHlluHrvoYyMBI/1Hms6QRq80TxKJRSk/RlxFGDA+ZUQ2ljjm0kc//Z8WjYe8rXravlrgiajqHvx+5wSENUIQyw=
!
ipv4 pool p4 2.2.2.1 0.0.0.1 254
!
vrf definition v1
 rd 1:1
 exit
!
interface loopback0
 no description
 vrf forwarding v1
 ipv4 address 4.4.4.4 255.255.255.255
 no shutdown
 no log-link-change
 exit
!
interface dialer1
 no description
 encapsulation ppp
 ppp ip4cp open
 ppp ip4cp local 2.2.2.0
 vrf forwarding v1
 ipv4 address 2.2.2.0 255.255.255.255
 ipv4 pool p4
 no shutdown
 no log-link-change
 exit
!
interface serial1
 no description
 encapsulation hdlc
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
server pckotcp pou
 security protocol tls
 security rsakey r
 security dsakey d
 security ecdsakey ed
 security rsacert r
 security dsacert d
 security ecdsacert ed
 clone dialer1
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
prefix-list p1
 sequence 10 permit 0.0.0.0/0 ge 0 le 0
 exit
!
vrf definition v1
 rd 1:1
 exit
!
interface dialer1
 no description
 encapsulation ppp
 ppp ip4cp open
 ppp ip4cp local 0.0.0.0
 vrf forwarding v1
 ipv4 address 3.3.3.3 255.255.255.128
 ipv4 gateway-prefix p1
 no shutdown
 no log-link-change
 exit
!
interface serial1
 no description
 encapsulation hdlc
 vrf forwarding v1
 ipv4 address 1.1.1.2 255.255.255.0
 ipv6 address 1234::2 ffff::
 no shutdown
 no log-link-change
 exit
!
proxy-profile p1
 security tls
 vrf v1
 exit
!
vpdn pou
 interface dialer1
 proxy p1
 target 1.1.1.1
 vcid 2554
 protocol pckotcp
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

## **Verification**
