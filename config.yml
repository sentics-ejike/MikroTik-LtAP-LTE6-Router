/ip ipsec profile
add dh-group=modp1024 enc-algorithm=aes-256 hash-algorithm=sha256 lifetime=8h name="Azure"



/ip ipsec peer
add address=4.185.9.150 exchange-mode=ike2 local-address=192.168.88.1 name="Azure" profile="Azure"


/ip ipsec proposal
add auth-algorithms=sha256 enc-algorithms=aes-256-cbc lifetime=7h30m name="Azure"


/ip firewall filter
add action=accept chain=input comment="Router fw input accept all active" connection-state=established,related,untracked
add action=accept chain=input comment="Azure access to router" dst-address=192.168.88.0/24 in-interface-list=WAN ipsec-policy=in,ipsec src-address=10.100.5.0/24
add action=drop chain=input comment="Router fw input drop invalid" connection-state=invalid
add action=drop chain=input comment="Router fw input drop all not from LAN" in-interface-list=!LAN
add action=accept chain=forward comment="Router fw IPsec in accept" ipsec-policy=in,ipsec
add action=accept chain=forward comment="Router fw IPsec out accept" ipsec-policy=out,ipsec
add action=fasttrack-connection chain=forward comment="Router fw forward fasttrack" connection-state=established,related
add action=accept chain=forward comment="Router fw forward accept all active" connection-state=established,related,untracked
add action=drop chain=forward comment="Router fw forward drop invalid" connection-state=invalid
add action=drop chain=forward comment="Router fw forward drop all from WAN not dstnated" connection-nat-state=!dstnat connection-state=new in-interface-list=WAN

/ip firewall mangle
add action=change-mss chain=forward comment="Azure" dst-address=10.100.5.0/24 new-mss=1350 passthrough=yes protocol=tcp tcp-flags=syn

/ip firewall nat
add action=accept chain=srcnat comment="Azure" dst-address=10.100.5.0/24 src-address=192.168.88.0/24
add action=masquerade chain=srcnat comment="Router fw masquerade" ipsec-policy=out,none out-interface-list=WAN

/ip ipsec identity
add peer="Azure" secret="H4%AQo4qHoRfNR$x8KY=o8Ew"

/ip ipsec policy
add dst-address=10.100.5.0/24 peer="Azure" proposal="Azure" sa-dst-address=4.185.9.150 sa-src-address=109.42.114.204 src-address=192.168.88.0/24 tunnel=yes
