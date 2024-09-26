
# SD1
```ciscoIOS
enable
conf t
!Asignar un nombre distintivo al dispositivo
hostname SD1
!Asignar un dominio de trabajo
ip domain-name lrc2.uaz.mx
!Deshabilitar la resolucion de nombres
no ip domain-lookup
!Diseniar un banner o una etiqueta de presentacion
banner motd $
[+]-----------------------------------------------[+]
 | LabRedes2 - CCNAv7 SRWE                         |
 | Ejercicio Integrador M3,M4,M5 y M6              |
 | 19 de septiembre de 2024                        |
 | SD1                                             |
 | Acceso restringido !!!                          |
[+]-----------------------------------------------[+]
$
!Crear una base de datos local para usuario
username admin secret cisco123$
!Asignar una password al nivel privilegiado
enable secret cla$$123
!Habilitar el acceso por SSH
! 1. Generar una llave para el cifrado de datos
crypto key generate rsa
1024
! 2. Habilitar el ssh version 2
ip ssh version 2
! 3. Configurar las lineas de acceso (admin), consola y terminales virtuales
line console 0
! - Se use la base de datos local para acceso
login local
! - Se sincronizen los mensajes a consola
loggin synchronous
! - Tiempo de actividad 
exec-timeout 15
! - Lineas virtuales (vty)
line vty 0 15
login local
exec-timeout 15
! - Habilitar el acceso con ssh
transport input ssh
service password-encryption

!conf de vlans
conf t
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
end

!conf de interfaces
conf t
interface range gi1/0/1-24
switchport mode trunk
switchport acces vlan 666
switchport trunk native vlan 1000
switchport trunk allowed vlan 32,64,128,1000

interface range gi1/1/1-4
switchport mode trunk
switchport acces vlan 666
switchport trunk native vlan 1000
switchport trunk allowed vlan 32,64,128,1000

exit
interface vlan 32
ip address 192.168.254.36 255.255.255.224
no shutdown

exit
ip default-gateway 192.168.254.33
end

```

# Router 

```ciscoIOS
enable
conf t

!Asignar un nombre distintivo al dispositivo
hostname R1

!Asignar un dominio de trabajo
ip domain-name s10.lrc2.uaz.mx

!Deshabilitar la resolucion de nombres
no ip domain-lookup
service password-encryption

!Diseniar un banner o una etiqueta de presentacion
banner motd $
[+]-----------------------------------------------[+]
 | LabRedes2 - CCNAv7 SRWE                         |
 | Ejercicio Integrador M3,M4,M5 y M6              |
 | 19 de septiembre de 2024                        |
 | R1                                              |
 | Acceso restringido !!!                          |
[+]-----------------------------------------------[+]
$
!Crear una base de datos local para usuario
username admin secret cisco123$

!Asignar una password al nivel privilegiado
enable secret cla$$123

!Habilitar el acceso por SSH
! 1. Generar una llave para el cifrado de datos
crypto key generate rsa
1024

! 2. Habilitar el ssh version 2
ip ssh version 2

! 3. Configurar las lineas de acceso (admin), consola y terminales virtuales
line console 0
! - Se use la base de datos local para acceso
login local
! - Se sincronizen los mensajes a consola
loggin synchronous
! - Tiempo de actividad 
exec-timeout 15
! - Lineas virtuales (vty)
line vty 0 4
login local
exec-timeout 15

! - Habilitar el acceso con ssh
transport input ssh


!un router no requiere crear vlans
interface gi0/1.128 
encapsulation dot1q 128
ip address 192.168.254.129 255.255.255.128

interface gi0/1.64
encapsulation dot1q 64
ip address 192.168.254.65 255.255.255.192

interface gi0/1.32
encapsulation dot1q 32
ip address 192.168.254.33 255.255.255.224

interface gi0/1.1000
encapsulation dot1q 1000 native
!la nativa no tiene ip asociada
!las subinterfaces no se encienden

interface gi0/1
no shutdown
```

