## Overview

> FIBER -> ONT -> eth0

> Gateway -> eth5 (sfp)

> swtich0 (eth[1-4]) -> LAN


### Notes:

to delete/add static mapping for dhcp

```bash
delete service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 static-mapping DEVICENAME
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 static-mapping DEVICENAME ip-address 192.168.1.XXX
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 static-mapping DEVICENAME mac-address aa:bb:cc:dd:ee:ff
```

port forwarding:

```
set port-forward auto-firewall disable
set port-forward hairpin-nat enable
set port-forward lan-interface switch0
set port-forward wan-interface eth5


set port-forward rule 100 description plex
set port-forward rule 100 forward-to address 192.168.1.24
set port-forward rule 100 forward-to port 32400          
set port-forward rule 100 original-port 32400  
set port-forward rule 100 protocol tcp_udp   

set port-forward rule 110 description flask
set port-forward rule 110 forward-to address 192.168.1.42
set port-forward rule 110 forward-to port 5000
set port-forward rule 110 original-port 5000
set port-forward rule 110 protocol tcp_udp
commit
```






# orignal script (with changes):

```
set firewall all-ping enable
set firewall broadcast-ping disable
set firewall ipv6-receive-redirects disable
set firewall ipv6-src-route disable
set firewall ip-src-route disable
set firewall log-martians enable

set firewall name WAN_IN default-action drop
set firewall name WAN_IN description 'WAN to internal'
set firewall name WAN_IN rule 10 action accept
set firewall name WAN_IN rule 10 description 'Allow established/related'
set firewall name WAN_IN rule 10 state established enable
set firewall name WAN_IN rule 10 state related enable
set firewall name WAN_IN rule 20 action drop
set firewall name WAN_IN rule 20 description 'Drop invalid state'
set firewall name WAN_IN rule 20 state invalid enable
```

set firewall for plex and flask app (my addition):

```
set firewall name WAN_IN rule 100 action accept 
set firewall name WAN_IN rule 100 description plex
set firewall name WAN_IN rule 100 destination address 192.168.1.24
set firewall name WAN_IN rule 100 destination port 32400          
set firewall name WAN_IN rule 100 log disable           
set firewall name WAN_IN rule 100 protocol tcp_udp 

set firewall name WAN_IN rule 110 action accept
set firewall name WAN_IN rule 110 description flask
set firewall name WAN_IN rule 110 destination address 192.168.1.42
set firewall name WAN_IN rule 110 destination port 5000
set firewall name WAN_IN rule 110 log disable
set firewall name WAN_IN rule 110 protocol tcp_udp
commit
```

# orignal script (with changes):

```
set firewall name WAN_LOCAL default-action drop
set firewall name WAN_LOCAL description 'WAN to router'
set firewall name WAN_LOCAL rule 10 action accept
set firewall name WAN_LOCAL rule 10 description 'Allow established/related'
set firewall name WAN_LOCAL rule 10 state established enable
set firewall name WAN_LOCAL rule 10 state related enable
set firewall name WAN_LOCAL rule 20 action drop
set firewall name WAN_LOCAL rule 20 description 'Drop invalid state'
set firewall name WAN_LOCAL rule 20 state invalid enable

set firewall receive-redirects disable
set firewall send-redirects enable
set firewall source-validation disable
set firewall syn-cookies enable
commit

set interfaces ethernet eth0 description WAN
set interfaces ethernet eth0 firewall in name WAN_IN
set interfaces ethernet eth0 firewall local name WAN_LOCAL
set interfaces ethernet eth0 vif 0 address dhcp
set interfaces ethernet eth0 vif 0 description 'WAN VLAN 0'
set interfaces ethernet eth0 vif 0 dhcp-options default-route update
set interfaces ethernet eth0 vif 0 dhcp-options default-route-distance 210
set interfaces ethernet eth0 vif 0 dhcp-options name-server update
set interfaces ethernet eth0 vif 0 firewall in name WAN_IN
set interfaces ethernet eth0 vif 0 firewall local name WAN_LOCAL
set interfaces ethernet eth0 vif 0 mac e0:22:04:86:c9:80
set interfaces ethernet eth5 description 'ATT Gateway'
set interfaces switch switch0 description Local
set interfaces switch switch0 switch-port interface eth1
set interfaces switch switch0 switch-port interface eth2
set interfaces switch switch0 switch-port interface eth3
set interfaces switch switch0 switch-port interface eth4
set interfaces switch switch0 switch-port vlan-aware disable
commit

set service dhcp-server disabled false
set service dhcp-server hostfile-update disable
set service dhcp-server shared-network-name LAN authoritative enable
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 default-router 192.168.1.1
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 dns-server 192.168.1.24
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 domain-name local
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 lease 86400
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 start 192.168.1.200 stop 192.168.1.209
set service dhcp-server static-arp disable
set service dhcp-server use-dnsmasq disable
commit
```




Static Mapping (formatting needs to be fixed):


```
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 static-mapping appleTV-master ip-address 192.168.1.53
set service dhcp-server shared-network-name LAN subnet 192.168.1.0/24 static-mapping appleTV-master mac-address 9c:20:7b:ea:60:cd


                static-mapping Galaxy-Tab-S2 {
                    ip-address 192.168.1.193
                    mac-address d0:87:e2:40:d4:a2
                }
                
                static-mapping Google-Mini-BedRoom {
                    ip-address 192.168.1.134
                    mac-address 7c:2e:bd:60:5c:70
                }
                
                static-mapping Google-Mini-GuestRoom {
                    ip-address 192.168.1.135
                    mac-address 44:07:0b:ca:d0:3c
                }
                
                static-mapping Google-Mini-Office {
                    ip-address 192.168.1.131
                    mac-address 7c:2e:bd:e1:9d:2d
                }
                
                static-mapping HDHomeRun-OTA-Recorder {
                    ip-address 192.168.1.58
                    mac-address 00:18:dd:05:8c:76
                }
                
                static-mapping NVR-P {
                    ip-address 192.168.1.240
                    mac-address 9c:8e:cd:0d:03:d1
                }
                
                static-mapping Nest-Downstairs {
                    ip-address 192.168.1.122
                    mac-address 64:16:66:9a:ca:7e
                }
                
                static-mapping TCLRoku {
                    ip-address 192.168.1.50
                    mac-address 3c:59:1e:f3:21:a3
                }
                
                static-mapping TCLRoku-WiFi {
                    ip-address 192.168.1.57
                    mac-address 0c:62:a6:03:df:40
                }
                
                static-mapping WiiU {
                    ip-address 192.168.1.51
                    mac-address 00:ee:22:aa:28:4d
                }
                
                static-mapping XboxOne {
                    ip-address 192.168.1.52
                    mac-address c4:9d:ed:35:00:9d
                }
                
                static-mapping XboxOne-WiFi {
                    ip-address 192.168.1.56
                    mac-address c4:9d:ed:35:00:9f
                }
                
                static-mapping bedroom-apple-tv-lan {
                    ip-address 192.168.1.59
                    mac-address d0:03:4b:22:ba:8a
                }
                
                static-mapping camera_backdoor {
                    ip-address 192.168.1.243
                    mac-address 9c:8e:cd:0c:dc:b9
                }
                
                static-mapping camera_cars {
                    ip-address 192.168.1.244
                    mac-address 9c:8e:cd:13:ed:e2
                }
                
                static-mapping camera_frontdoor {
                    ip-address 192.168.1.242
                    mac-address 9c:8e:cd:0c:dc:44
                }
                
                static-mapping camera_hector {
                    ip-address 192.168.1.245
                    mac-address 9c:8e:cd:0c:dc:0d
                }
                
                static-mapping camera_patio {
                    ip-address 192.168.1.241
                    mac-address 9c:8e:cd:0e:fa:84
                }
                
                static-mapping chromecast {
                    ip-address 192.168.1.54
                    mac-address 6c:ad:f8:33:99:78
                }
                
                static-mapping docker {
                    ip-address 192.168.1.24
                    mac-address 00:0c:29:7a:57:b1
                }
                
                static-mapping echo1 {
                    ip-address 192.168.1.127
                    mac-address 44:65:0d:36:b4:c5
                }
                
                static-mapping echo2 {
                    ip-address 192.168.1.128
                    mac-address 74:c2:46:7e:28:d8
                }
                
                static-mapping entertainment-switch {
                    ip-address 192.168.1.13
                    mac-address 98:de:d0:df:32:83
                }
                
                static-mapping esxi {
                    ip-address 192.168.1.20
                    mac-address 00:24:e8:63:65:89
                }
                
                static-mapping esxi-windows {
                    ip-address 192.168.1.26
                    mac-address 00:0c:29:d2:5d:b3
                }
                
                static-mapping files {
                    ip-address 192.168.1.25
                    mac-address 00:0c:29:5f:18:32
                }
                
                static-mapping google-mini-dinningroom {
                    ip-address 192.168.1.136
                    mac-address 38:8b:59:34:d2:12
                }
                
                static-mapping google-mini-kitchen {
                    ip-address 192.168.1.133
                    mac-address 20:df:b9:cc:ef:84
                }
                
                static-mapping google-mini-livingroom {
                    ip-address 192.168.1.132
                    mac-address b0:2a:43:15:6c:2d
                }
                
                static-mapping guestroom-apple-tv {
                    ip-address 192.168.1.55
                    mac-address d0:03:4b:22:ba:88
                }
                
                static-mapping idrac-r710 {
                    ip-address 192.168.1.29
                    mac-address 00:24:e8:63:65:91
                }
                
                static-mapping jarvis-lan {
                    ip-address 192.168.1.45
                    mac-address 5c:26:0a:16:fc:a0
                }
                
                static-mapping jarvis-wlan {
                    ip-address 192.168.1.46
                    mac-address 3c:a9:f4:5d:54:a8
                }
                
                static-mapping nest-upstairs {
                    ip-address 192.168.1.125
                    mac-address 18:b4:30:c8:d5:eb
                }
                
                static-mapping network-switch {
                    ip-address 192.168.1.11
                    mac-address 98:de:d0:7a:ca:14
                }
                
                static-mapping office-switch {
                    ip-address 192.168.1.12
                    mac-address 18:a6:f7:06:b5:af
                }
                
                static-mapping office_printer {
                    ip-address 192.168.1.41
                    mac-address 30:05:5c:16:17:f9
                }
                
                static-mapping patrick-nexus {
                    ip-address 192.168.1.190
                    mac-address 64:bc:0c:4c:3f:3b
                }
                
                static-mapping patrickWorkPhone {
                    ip-address 192.168.1.191
                    mac-address 10:98:c3:c4:98:d2
                }
                
                static-mapping patrick-pixel-3 {
                    ip-address 192.168.1.196
                    mac-address 58:cb:52:15:47:11
                }
                
                static-mapping patrick-work-tablet {
                    ip-address 192.168.1.195
                    mac-address d4:11:a3:dc:fe:27
                }
                
                static-mapping pixelbook-go {
                    ip-address 192.168.1.197
                    mac-address 4c:1d:96:f0:c8:8c
                }
                
                static-mapping pizero_keyfob {
                    ip-address 192.168.1.42
                    mac-address b8:27:eb:6d:5e:76
                }
                
                static-mapping raspberrypi {
                    ip-address 192.168.1.40
                    mac-address b8:27:eb:46:c4:2f
                }
                
                static-mapping trisha-desktop {
                    ip-address 192.168.1.44
                    mac-address 18:66:da:06:83:14
                }
                
                static-mapping trisha-moto-del {
                    ip-address 192.168.1.192
                    mac-address 88:b4:a6:3d:a3:05
                }
                
                static-mapping trisha-pixel-3 {
                    ip-address 192.168.1.194
                    mac-address 3c:28:6d:e4:87:3e
                }
                
                static-mapping unifi-ap {
                    ip-address 192.168.1.5
                    mac-address 80:2a:a8:59:54:53
                }
                
                static-mapping unificont {
                    ip-address 192.168.1.23
                    mac-address 00:0c:29:7e:b6:a6
                }
                
                static-mapping wemo1 {
                    ip-address 192.168.1.123
                    mac-address 58:ef:68:9a:5e:bd
                }
                
                static-mapping wemo2 {
                    ip-address 192.168.1.124
                    mac-address 94:10:3e:30:c5:b1
                }
                
                static-mapping wemoAirCompressor {
                    ip-address 192.168.1.129
                    mac-address 24:f5:a2:f3:8d:cb
                }
                
                static-mapping wemoWhiteNoise {
                    ip-address 192.168.1.126
                    mac-address 24:f5:a2:1b:ed:2f
                }
                
                unifi-controller 192.168.1.23
            }
        }
```

# orignal script (with changes):


```
set service dns forwarding cache-size 300
set service dns forwarding listen-on switch0


set service dns forwarding name-server 192.168.1.24
set service dns forwarding name-server 8.8.8.8
set service dns forwarding options listen-address=192.168.1.24


set service nat rule 5010 description 'masquerade for WAN'
set service nat rule 5010 outbound-interface eth0.0
set service nat rule 5010 type masquerade


set system offload hwnat enable

delete interfaces ethernet eth0 address

set interfaces switch switch0 address 192.168.1.1/24

commit;save
```
