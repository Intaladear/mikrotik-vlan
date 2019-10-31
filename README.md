# mikrotik-vlan
mikrotik-vlan

# jan/02/1970 09:29:06 by RouterOS 6.42.6
# software id = S17A-SCPK
#
# model = RouterBOARD 931-2nD
# serial number = 7CCA08FD9370
/interface bridge
add disabled=yes fast-forward=no name=bridge-vlan101
add disabled=yes fast-forward=no name=bridge-vlan102
add disabled=yes fast-forward=no name=bridge-vlan200
add fast-forward=no name=bridge1 vlan-filtering=yes
/interface vlan
add disabled=yes interface=ether2 name=eth2-vlan101 vlan-id=101
add disabled=yes interface=ether2 name=eth2-vlan102 vlan-id=102
add disabled=yes interface=ether2 name=eth2-vlan200 vlan-id=200
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
add authentication-types=wpa-psk,wpa2-psk eap-methods="" \
    management-protection=allowed mode=dynamic-keys name=TEST \
    supplicant-identity="" wpa-pre-shared-key=0814014211 wpa2-pre-shared-key=\
    0814014211
/interface wireless
set [ find default-name=wlan1 ] disabled=no mode=ap-bridge security-profile=\
    TEST ssid=MikroTik
/interface bridge port
add bridge=bridge1 interface=ether1
add bridge=bridge1 interface=ether2 pvid=101
add bridge=bridge1 interface=ether3 pvid=200
add bridge=bridge1 disabled=yes interface=eth2-vlan101 pvid=101
add bridge=bridge1 disabled=yes interface=eth2-vlan102 pvid=102
add bridge=bridge-vlan200 disabled=yes interface=eth2-vlan200 pvid=200
add bridge=bridge1 interface=wlan1
/interface bridge vlan
add bridge=bridge1 tagged=ether1,ether2 untagged=ether3 vlan-ids=200
add bridge=bridge1 tagged=ether1,ether2 vlan-ids=101
add bridge=bridge1 tagged=ether1,ether2 vlan-ids=102
/ip dhcp-client
add dhcp-options=hostname,clientid disabled=no interface=bridge1
/ip dns
set allow-remote-requests=yes servers=8.8.8.8,8.4.4.4
/ip firewall nat
add action=masquerade chain=srcnat
/ip route
add distance=1 gateway=ether1
/system identity
set name=SW1
/system routerboard settings
set silent-boot=no
/tool romon
set enabled=yes
