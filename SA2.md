```ciscoIOS
enable
conf t
hostname SA2
ip domain-name lrc2.uaz.mx
no ip domain-lookup
banner motd $
[+]------------------------------------[+]
 | LabRedes2 - CCNAv7 SRWE              |
 | EjercicioIntegrador M3, M4, M5 y M6  | 
 | 19 de septiembre  2024               |
 | SA2                                  |
 | MAYOGI                               |
 | Acceso restringido !!!               |
[+]------------------------------------[+]
$
username admin secret cisco123$
enable secret cla$$123
crypto key generate rsa
1024
ip ssh version 2
line console 0
login local
loggin synchronous
exec-timeout 15
line vty 0 15
login local
exec-timeout 15
transport input ssh
service password-encryption


! Vlans
vlan 128 
name Development
 
vlan 64
name Design

vlan 32
name IT

vlan 1000
name NATIVA

vlan 666
name ParkingLot



!-- Asignación de los puertos a las VLANs ---
interface range fa0/5-9
switchport mode access
switchport access vlan 128

interface range fa0/10-14
switchport mode access
switchport access vlan 64

interface range fa0/15-19
switchport mode access
switchport access vlan 32

interface range fa0/20-24,gi0/1-2
switchport mode dynamic auto 
switchport trunk native vlan 1000
switchport trunk allowed vlan 128,64,32
switchport access vlan 666
!switchport trunk allowed vlan 666

interface range fa0/1-4
switchport mode access
switchport access vlan 666
shutdown

end 
interface vlan 32
ip address 192.168.254.35 255.255.255.224
no shutdown
exit

ip default-gateway 192.168.254.33 
end
```

