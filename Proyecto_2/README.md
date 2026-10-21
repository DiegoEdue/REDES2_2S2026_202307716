# Documentación Técnica Proyecto 2 - RC2 - 202307716

## 1. Topología Completa

### Topología en Packet Tracer

![topologia](./imgs/topologia.png)

### Topología en drawio

![topologia](./imgs/topologia_draw.svg)

## 2. Cálculos de Subnetting (VLSM/FLSM)

### 2.1 VLSM

**Calculadora utilizada para realizar los cálculos de VLSM:**
[Calculadora VLSM](https://www.arcadio.gq/calculadora-subredes-vlsm.html)

![VLSM](./imgs/vlsm.png)

![VLSM1](./imgs/vlsm1.png)

![VLSM2](./imgs/vlsm2.png)

#### 2.1.1 ISP1: FibraNet GT

| **VLAN**     | **ID VLAN** | **No. de Hosts** | **IP de Red**     | **Máscara**     | **Wildcard** | **Primer Host** | **Último Host** | **Broadcast** |
| :----------- | :---------- | :--------------- | :---------------- | :-------------- | :----------- | :-------------- | :-------------- | :------------ |
| VLAN_ADMIN   | 16          | 126              | 172.16.16.0 /25   | 255.255.255.128 | 0.0.0.127    | 172.16.16.1     | 172.16.16.126   | 172.16.16.127 |
| VLAN_CLIENTE | 26          | 126              | 172.16.16.128 /25 | 255.255.255.128 | 0.0.0.127    | 172.16.16.129   | 172.16.16.254   | 172.16.16.255 |

#### 2.1.2 ISP2: AndinaCom

| **VLAN**         | **ID VLAN** | **No. de Hosts** | **IP de Red**    | **Máscara**     | **Wildcard** | **Primer Host** | **Último Host** | **Broadcast** |
| :--------------- | :---------- | :--------------- | :--------------- | :-------------- | :----------- | :-------------- | :-------------- | :------------ |
| VLAN_VENTAS      | 36          | 60               | 172.16.26.0 /26  | 255.255.255.192 | 0.0.0.63     | 172.16.26.1     | 172.16.26.62    | 172.16.26.63  |
| VLAN_FACTURACION | 46          | 60               | 172.16.26.64 /26 | 255.255.255.192 | 0.0.0.63     | 172.16.26.65    | 172.16.26.126   | 172.16.26.127 |

#### 2.1.3 ISP3: MoonLink

| **VLAN**       | **ID VLAN** | **No. de Hosts** | **IP de Red**    | **Máscara**     | **Wildcard** | **Primer Host** | **Último Host** | **Broadcast** |
| :------------- | :---------- | :--------------- | :--------------- | :-------------- | :----------- | :-------------- | :-------------- | :------------ |
| VLAN_SOPORTE   | 56          | 62               | 172.16.32.0 /26  | 255.255.255.192 | 0.0.0.63     | 172.16.32.1     | 172.16.32.62    | 172.16.32.63  |
| VLAN_SEGURIDAD | 66          | 62               | 172.16.32.64 /26 | 255.255.255.192 | 0.0.0.63     | 172.16.32.65    | 172.16.32.126   | 172.16.32.127 |

### 2.2 FLSM

**Calculadora utilizada para realizar los cálculos de FLSM:**
[Calculadora VLSM](https://www.arcadio.gq/calculadora-subredes-flsm.html)

![FLSM](./imgs/flsm.png)

![FLSM1](./imgs/flsm1.png)

![FLSM2](./imgs/flsm2.png)

![FLSM3](./imgs/flsm3.png)

![FLSM4](./imgs/flsm4.png)

![FLSM5](./imgs/flsm5.png)

![FLSM6](./imgs/flsm6.png)

#### 2.2.1 Tabla de FLSM

| **IP de Red**     | **Máscara**     | **Wildcard** | **Primer Host** | **Último Host** | **IP Broadcast** |
| :---------------- | :-------------- | :----------- | :-------------- | :-------------- | :--------------- |
| 192.168.16.0 /30  | 255.255.255.252 | 0.0.0.3      | 192.168.16.1    | 192.168.16.2    | 192.168.16.3     |
| 192.168.16.4 /30  | 255.255.255.252 | 0.0.0.3      | 192.168.16.5    | 192.168.16.6    | 192.168.16.7     |
| 192.168.16.8 /30  | 255.255.255.252 | 0.0.0.3      | 192.168.16.9    | 192.168.16.10   | 192.168.16.11    |
| 192.168.16.12 /30 | 255.255.255.252 | 0.0.0.3      | 192.168.16.13   | 192.168.16.14   | 192.168.16.15    |
| 192.168.16.16 /30 | 255.255.255.252 | 0.0.0.3      | 192.168.16.17   | 192.168.16.18   | 192.168.16.19    |
| 192.168.16.20 /30 | 255.255.255.252 | 0.0.0.3      | 192.168.16.21   | 192.168.16.22   | 192.168.16.23    |
| 192.168.16.24 /30 | 255.255.255.252 | 0.0.0.3      | 192.168.16.25   | 192.168.16.26   | 192.168.16.27    |

## 3. Configuraciones

### 3.1 ISP 1: FibraNet GT

#### Multilayer Switch 3650 - MS1

```bash
enable
configure terminal
hostname MS1
no ip domain-lookup
ip routing

! Enlaces inter-ISP (fibra)
interface GigabitEthernet1/1/1
 description BGP a MS2 (AndinaCom)
 no switchport
 ip address 192.168.16.1 255.255.255.252
 no shutdown

interface GigabitEthernet1/1/2
 description BGP a MS3 (MoonLink)
 no switchport
 ip address 192.168.16.9 255.255.255.252
 no shutdown

! Enlace interno hacia R1
interface GigabitEthernet1/0/1
 description Enlace a R1
 no switchport
 ip address 192.168.16.17 255.255.255.252
 no shutdown

router ospf 1
 router-id 1.1.1.1
 network 192.168.16.16 0.0.0.3 area 0
 passive-interface GigabitEthernet1/1/1
 passive-interface GigabitEthernet1/1/2
 redistribute bgp 100 subnets

router bgp 100
 bgp router-id 1.1.1.1
 bgp log-neighbor-changes
 neighbor 192.168.16.2 remote-as 200
 neighbor 192.168.16.10 remote-as 300
 network 172.16.16.0 mask 255.255.255.128
 network 172.16.16.128 mask 255.255.255.128
end
write
```

#### Router 2911 - R1

```bash
enable
vlan database
 vlan 16 name VLAN_ADMIN
 vlan 26 name VLAN_CLIENTE
 exit
configure terminal
hostname R1
no ip domain-lookup

ip access-list extended ACL_ATENCION_IN
 permit icmp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 established
 deny ip 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 deny ip 172.16.16.128 0.0.0.127 172.16.26.64 0.0.0.63
 permit ip any any
exit

interface GigabitEthernet0/0
 description Enlace a MS1
 ip address 192.168.16.18 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Enlace a RI1
 ip address 192.168.16.21 255.255.255.252
 no shutdown

interface GigabitEthernet0/2
 description Enlace a RD1
 ip address 192.168.16.25 255.255.255.252
 no shutdown

interface FastEthernet0/3/0
 description Trunk hacia SW1 (VLAN 16 y 26)
 switchport mode trunk
 no shutdown

interface Vlan16
 description Gateway ADMIN (HSRP)
 ip address 172.16.16.2 255.255.255.128
 ip helper-address 172.16.26.140
 standby 16 ip 172.16.16.1
 standby 16 priority 120
 standby 16 preempt
 no shutdown

interface Vlan26
 description Gateway ATENCION (HSRP)
 ip address 172.16.16.252 255.255.255.128
 ip helper-address 172.16.26.140
 ip access-group ACL_ATENCION_IN in
 standby 26 ip 172.16.16.129
 standby 26 priority 100
 standby 26 preempt
 no shutdown

router ospf 1
 router-id 1.1.2.1
 network 192.168.16.16 0.0.0.3 area 0
 network 192.168.16.20 0.0.0.3 area 0
 network 192.168.16.24 0.0.0.3 area 0
 network 172.16.16.0 0.0.0.127 area 0
 network 172.16.16.128 0.0.0.127 area 0
 passive-interface Vlan16
 passive-interface Vlan26
end
write
```

#### Router 2911 - RI1

```bash
enable
configure terminal
hostname RI1
no ip domain-lookup

ip access-list extended ACL_ATENCION_IN
 permit icmp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 established
 deny ip 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 deny ip 172.16.16.128 0.0.0.127 172.16.26.64 0.0.0.63
 permit ip any any
exit

interface GigabitEthernet0/1
 description Enlace a R1
 ip address 192.168.16.22 255.255.255.252
 no shutdown

interface GigabitEthernet0/0
 description Enlace a RI2
 ip address 192.168.16.29 255.255.255.252
 no shutdown

interface GigabitEthernet0/2
 description Trunk hacia SWI1 (VLAN 16 y 26)
 no ip address
 no shutdown

interface GigabitEthernet0/2.16
 description Gateway ADMIN (HSRP)
 encapsulation dot1Q 16
 ip address 172.16.16.5 255.255.255.128
 ip helper-address 172.16.26.140
 standby 16 ip 172.16.16.1
 standby 16 priority 90
 standby 16 preempt
 standby 16 track GigabitEthernet0/1 30

interface GigabitEthernet0/2.26
 description Gateway ATENCION (HSRP)
 encapsulation dot1Q 26
 ip address 172.16.16.250 255.255.255.128
 ip helper-address 172.16.26.140
 ip access-group ACL_ATENCION_IN in
 standby 26 ip 172.16.16.129
 standby 26 priority 120
 standby 26 preempt
 standby 26 track GigabitEthernet0/1 30

router ospf 1
 router-id 1.1.3.1
 network 192.168.16.20 0.0.0.3 area 0
 network 192.168.16.28 0.0.0.3 area 0
 network 172.16.16.0 0.0.0.127 area 0
 network 172.16.16.128 0.0.0.127 area 0
 passive-interface GigabitEthernet0/2.16
 passive-interface GigabitEthernet0/2.26
end
write
```

#### Router 2911 - RI2

```bash
enable
configure terminal
hostname RI2
no ip domain-lookup

ip access-list extended ACL_ATENCION_IN
 permit icmp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 established
 deny ip 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 deny ip 172.16.16.128 0.0.0.127 172.16.26.64 0.0.0.63
 permit ip any any
exit

interface GigabitEthernet0/0
 description Enlace a RI1
 ip address 192.168.16.30 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Trunk hacia SWI2 (VLAN 16 y 26)
 no ip address
 no shutdown

interface GigabitEthernet0/1.16
 description Gateway ADMIN (HSRP)
 encapsulation dot1Q 16
 ip address 172.16.16.6 255.255.255.128
 ip helper-address 172.16.26.140
 standby 16 ip 172.16.16.1
 standby 16 priority 80
 standby 16 preempt
 standby 16 track GigabitEthernet0/0 30

interface GigabitEthernet0/1.26
 description Gateway ATENCION (HSRP)
 encapsulation dot1Q 26
 ip address 172.16.16.251 255.255.255.128
 ip helper-address 172.16.26.140
 ip access-group ACL_ATENCION_IN in
 standby 26 ip 172.16.16.129
 standby 26 priority 110
 standby 26 preempt
 standby 26 track GigabitEthernet0/0 30

router ospf 1
 router-id 1.1.3.2
 network 192.168.16.28 0.0.0.3 area 0
 network 172.16.16.0 0.0.0.127 area 0
 network 172.16.16.128 0.0.0.127 area 0
 passive-interface GigabitEthernet0/1.16
 passive-interface GigabitEthernet0/1.26
end
write
```

#### Router 2911 - RD1

```bash
enable
configure terminal
hostname RD1
no ip domain-lookup

ip access-list extended ACL_ATENCION_IN
 permit icmp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 established
 deny ip 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 deny ip 172.16.16.128 0.0.0.127 172.16.26.64 0.0.0.63
 permit ip any any
exit

interface GigabitEthernet0/0
 description Enlace a R1
 ip address 192.168.16.26 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Enlace a RD2
 ip address 192.168.16.33 255.255.255.252
 no shutdown

interface GigabitEthernet0/2
 description Trunk hacia SWD1 (VLAN 16 y 26)
 no ip address
 no shutdown

interface GigabitEthernet0/2.16
 description Gateway ADMIN (HSRP)
 encapsulation dot1Q 16
 ip address 172.16.16.3 255.255.255.128
 ip helper-address 172.16.26.140
 standby 16 ip 172.16.16.1
 standby 16 priority 110
 standby 16 preempt
 standby 16 track GigabitEthernet0/0 30

interface GigabitEthernet0/2.26
 description Gateway ATENCION (HSRP)
 encapsulation dot1Q 26
 ip address 172.16.16.253 255.255.255.128
 ip helper-address 172.16.26.140
 ip access-group ACL_ATENCION_IN in
 standby 26 ip 172.16.16.129
 standby 26 priority 90
 standby 26 preempt
 standby 26 track GigabitEthernet0/0 30

router ospf 1
 router-id 1.1.4.1
 network 192.168.16.24 0.0.0.3 area 0
 network 192.168.16.32 0.0.0.3 area 0
 network 172.16.16.0 0.0.0.127 area 0
 network 172.16.16.128 0.0.0.127 area 0
 passive-interface GigabitEthernet0/2.16
 passive-interface GigabitEthernet0/2.26
end
write
```

#### Router 2911 - RD2

```bash
enable
configure terminal
hostname RD2
no ip domain-lookup

ip access-list extended ACL_ATENCION_IN
 permit icmp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63 established
 deny ip 172.16.16.128 0.0.0.127 172.16.26.0 0.0.0.63
 deny ip 172.16.16.128 0.0.0.127 172.16.26.64 0.0.0.63
 permit ip any any
exit

interface GigabitEthernet0/0
 description Enlace a RD1
 ip address 192.168.16.34 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Trunk hacia SWD3 (VLAN 16 y 26)
 no ip address
 no shutdown

interface GigabitEthernet0/1.16
 description Gateway ADMIN (HSRP)
 encapsulation dot1Q 16
 ip address 172.16.16.4 255.255.255.128
 ip helper-address 172.16.26.140
 standby 16 ip 172.16.16.1
 standby 16 priority 100
 standby 16 preempt
 standby 16 track GigabitEthernet0/0 30

interface GigabitEthernet0/1.26
 description Gateway ATENCION (HSRP)
 encapsulation dot1Q 26
 ip address 172.16.16.254 255.255.255.128
 ip helper-address 172.16.26.140
 ip access-group ACL_ATENCION_IN in
 standby 26 ip 172.16.16.129
 standby 26 priority 80
 standby 26 preempt
 standby 26 track GigabitEthernet0/0 30

router ospf 1
 router-id 1.1.4.2
 network 192.168.16.32 0.0.0.3 area 0
 network 172.16.16.0 0.0.0.127 area 0
 network 172.16.16.128 0.0.0.127 area 0
 passive-interface GigabitEthernet0/1.16
 passive-interface GigabitEthernet0/1.26
end
write
```

#### Switch 2960 - SW1

```bash
enable
configure terminal
hostname SW1
no ip domain-lookup

vlan 16
 name VLAN_ADMIN
vlan 26
 name VLAN_CLIENTE
exit

interface FastEthernet0/1
 description Server DNS/HTTP
 switchport mode access
 switchport access vlan 16
 spanning-tree portfast

interface FastEthernet0/2
 description Trunk hacia R1 Fa0/3/0
 switchport mode trunk

interface FastEthernet0/4
 description Trunk hacia SWD1 Fa0/3
 switchport mode trunk

interface FastEthernet0/3
 description Trunk hacia SWI1 Fa0/3
 switchport mode trunk
end
write
```

#### Switch 2960 - SWI1

```bash
enable
configure terminal
hostname SWI1
no ip domain-lookup

vlan 16
 name VLAN_ADMIN
vlan 26
 name VLAN_CLIENTE
exit

interface GigabitEthernet0/1
 description Trunk (RI1)
 switchport mode trunk

interface range FastEthernet0/3 - 4
 description Trunk hacia (SW1 / SWI2)
 switchport mode trunk

interface FastEthernet0/1
 description Laptop4 (Verde - Atencion)
 switchport mode access
 switchport access vlan 26
 spanning-tree portfast

interface FastEthernet0/2
 description PC4 (Amarillo - Admin)
 switchport mode access
 switchport access vlan 16
 spanning-tree portfast
end
write
```

#### Switch 2960 - SWI2

```bash
enable
configure terminal
hostname SWI2
no ip domain-lookup

vlan 16
 name VLAN_ADMIN
vlan 26
 name VLAN_CLIENTE
exit

interface GigabitEthernet0/1
 description Trunk (RI2)
 switchport mode trunk

interface FastEthernet0/3
 description Trunk (SWI1)
 switchport mode trunk

interface FastEthernet0/1
 description Laptop5 (Amarillo - Admin)
 switchport mode access
 switchport access vlan 16
 spanning-tree portfast

interface FastEthernet0/2
 description PC5 (Verde - Atencion)
 switchport mode access
 switchport access vlan 26
 spanning-tree portfast
end
write
```

#### Switch 2960 - SWD1

```bash
enable
configure terminal
hostname SWD1
no ip domain-lookup

vlan 16
 name VLAN_ADMIN
vlan 26
 name VLAN_CLIENTE
exit

interface GigabitEthernet0/1
 description Trunk (RD1)
 switchport mode trunk

interface range FastEthernet0/3 - 4
 description Trunk hacia (SW1 / SWD3)
 switchport mode trunk

interface range FastEthernet0/1 - 2
 description LACP hacia SWD2
 switchport mode trunk
 channel-group 1 mode active
exit

interface Port-channel1
 switchport mode trunk
end
write
```

#### Switch 2960 - SWD2

```bash
enable
configure terminal
hostname SWD2
no ip domain-lookup

vlan 16
 name VLAN_ADMIN
vlan 26
 name VLAN_CLIENTE
exit

interface range FastEthernet0/3 - 4
 description LACP hacia SWD1
 switchport mode trunk
 channel-group 1 mode passive
exit

interface Port-channel1
 switchport mode trunk

interface FastEthernet0/1
 description PC7 (Verde - Atencion)
 switchport mode access
 switchport access vlan 26
 spanning-tree portfast

interface FastEthernet0/2
 description Laptop7 (Amarillo - Admin)
 switchport mode access
 switchport access vlan 16
 spanning-tree portfast
end
write
```

#### Switch 2960 - SWD3

```bash
enable
configure terminal
hostname SWD3
no ip domain-lookup

vlan 16
 name VLAN_ADMIN
vlan 26
 name VLAN_CLIENTE
exit

interface GigabitEthernet0/1
 description Trunk (RD2)
 switchport mode trunk

interface FastEthernet0/3
 description Trunk (SWD1)
 switchport mode trunk

interface range FastEthernet0/1 - 2
 description LACP hacia SWD4
 switchport mode trunk
 channel-group 1 mode active
exit

interface Port-channel1
 switchport mode trunk
end
write
```

#### Switch 2960 - SWD4

```bash
enable
configure terminal
hostname SWD4
no ip domain-lookup

vlan 16
 name VLAN_ADMIN
vlan 26
 name VLAN_CLIENTE
exit

interface range FastEthernet0/3 - 4
 description LACP hacia SWD3
 switchport mode trunk
 channel-group 1 mode passive
exit

interface Port-channel1
 switchport mode trunk

interface FastEthernet0/1
 description PC6 (Amarillo - Admin)
 switchport mode access
 switchport access vlan 16
 spanning-tree portfast

interface FastEthernet0/2
 description Laptop6 (Amarillo - Admin)
 switchport mode access
 switchport access vlan 16
 spanning-tree portfast
end
write
```

### Server (ServerWeb)

IP estática del servidor:

- IP: `172.16.16.10`
- Máscara: `255.255.255.128`
- Gateway: `172.16.16.1`
- DNS Server: `172.16.16.10`

#### Server HTTP y HTTPS

```html
<html>
  <head>
    <title>Proyecto 2</title>
  </head>
  <body style="text-align: center; font-family: Arial;">
    <h1>Datos:</h1>
    <p>Diego Eduardo España Vásquez - 202307716</p>
    <p>Redes de Computadoras 2 - N</p>
    <p>Proyecto Conectividad XXI</p>
  </body>
</html>
```

#### DNS Service

- Name: `www.proyecto2_202307716.com`
- Type: `A Record`
- Address: `172.16.16.10`

Hacer click en Add.

### 3.2 ISP 2: AndinaCom

#### Multilayer Switch 3650 - MS2

```bash
enable
configure terminal
hostname MS2
no ip domain-lookup
ip routing

! Enlaces inter-ISP (fibra)
interface GigabitEthernet1/1/2
 description BGP a MS1 (FibraNet GT)
 no switchport
 ip address 192.168.16.2 255.255.255.252
 no shutdown

interface GigabitEthernet1/1/1
 description BGP a MS3 (MoonLink)
 no switchport
 ip address 192.168.16.5 255.255.255.252
 no shutdown

! Segmento de servicios / core (hacia SW1-ISP2)
interface GigabitEthernet1/0/1
 description Segmento core y Server DHCP (SW1-ISP2)
 no switchport
 ip address 172.16.26.129 255.255.255.240
 no shutdown

router ospf 1
 router-id 2.2.0.1
 network 172.16.26.128 0.0.0.15 area 0
 passive-interface GigabitEthernet1/1/1
 passive-interface GigabitEthernet1/1/2
 redistribute bgp 200 subnets

router bgp 200
 bgp router-id 2.2.0.1
 bgp log-neighbor-changes
 neighbor 192.168.16.1 remote-as 100
 neighbor 192.168.16.6 remote-as 300
 network 172.16.26.0 mask 255.255.255.192
 network 172.16.26.64 mask 255.255.255.192
 network 172.16.26.128 mask 255.255.255.240
end
write
```

#### Switch 2960 - SW1-ISP2

```bash
enable
configure terminal
hostname SW1-ISP2
no ip domain-lookup

vlan 99
 name SERVIDORES_CORE
exit

interface GigabitEthernet0/1
 description Hacia MS2 Gi1/0/1
 switchport mode access
 switchport access vlan 99

interface FastEthernet0/1
 description Server DHCP
 switchport mode access
 switchport access vlan 99
 spanning-tree portfast

interface FastEthernet0/2
 description Hacia RI1 Gi0/0
 switchport mode access
 switchport access vlan 99

interface FastEthernet0/3
 description Hacia RD1 Gi0/0
 switchport mode access
 switchport access vlan 99
end
write
```

#### Router 2911 - RI1-ISP2

```bash
enable
configure terminal
hostname RI1-ISP2
no ip domain-lookup

interface GigabitEthernet0/0
 description Segmento core (SW1-ISP2)
 ip address 172.16.26.130 255.255.255.240
 no shutdown

interface GigabitEthernet0/2
 description Enlace a RI2-ISP2
 ip address 192.168.16.65 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Enlace a RI3-ISP2
 ip address 192.168.16.69 255.255.255.252
 no shutdown

router ospf 1
 router-id 2.2.1.1
 network 172.16.26.128 0.0.0.15 area 0
 network 192.168.16.64 0.0.0.3 area 0
 network 192.168.16.68 0.0.0.3 area 0
end
write
```

#### Router 2911 - RI2-ISP2

```bash
enable
configure terminal
hostname RI2-ISP2
no ip domain-lookup

ip access-list extended ACL_VENTAS_IN
 permit icmp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63
 permit tcp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63 eq 80
 permit tcp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63 established
 deny ip 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63
 permit icmp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127
 permit tcp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127 eq 80
 permit tcp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127 established
 deny ip 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127
 permit ip any any

ip access-list extended ACL_FACT_IN
 permit icmp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63
 permit tcp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63 established
 deny ip 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63
 deny ip 172.16.26.64 0.0.0.63 172.16.16.128 0.0.0.127
 permit ip any any
exit

interface GigabitEthernet0/0
 description Enlace a RI1-ISP2
 ip address 192.168.16.66 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Trunk a SW2-ISP2
 no ip address
 no shutdown

interface GigabitEthernet0/1.36
 description Gateway VENTAS (HSRP)
 encapsulation dot1Q 36
 ip address 172.16.26.2 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_VENTAS_IN in
 standby 36 ip 172.16.26.1
 standby 36 priority 120
 standby 36 preempt
 standby 36 track GigabitEthernet0/0 30

interface GigabitEthernet0/1.46
 description Gateway FACTURACION (HSRP)
 encapsulation dot1Q 46
 ip address 172.16.26.66 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_FACT_IN in
 standby 46 ip 172.16.26.65
 standby 46 priority 120
 standby 46 preempt
 standby 46 track GigabitEthernet0/0 30

router ospf 1
 router-id 2.2.1.2
 network 192.168.16.64 0.0.0.3 area 0
 network 172.16.26.0 0.0.0.63 area 0
 network 172.16.26.64 0.0.0.63 area 0
 passive-interface GigabitEthernet0/1.36
 passive-interface GigabitEthernet0/1.46
end
write
```

#### Router 2911 - RI3-ISP2

```bash
enable
configure terminal
hostname RI3-ISP2
no ip domain-lookup

ip access-list extended ACL_VENTAS_IN
 permit icmp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63
 permit tcp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63 eq 80
 permit tcp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63 established
 deny ip 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63
 permit icmp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127
 permit tcp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127 eq 80
 permit tcp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127 established
 deny ip 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127
 permit ip any any

ip access-list extended ACL_FACT_IN
 permit icmp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63
 permit tcp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63 established
 deny ip 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63
 deny ip 172.16.26.64 0.0.0.63 172.16.16.128 0.0.0.127
 permit ip any any
exit

interface GigabitEthernet0/0
 description Enlace a RI1-ISP2
 ip address 192.168.16.70 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Trunk a SW2-ISP2
 no ip address
 no shutdown

interface GigabitEthernet0/1.36
 description Gateway VENTAS (HSRP)
 encapsulation dot1Q 36
 ip address 172.16.26.3 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_VENTAS_IN in
 standby 36 ip 172.16.26.1
 standby 36 priority 110
 standby 36 preempt
 standby 36 track GigabitEthernet0/0 30

interface GigabitEthernet0/1.46
 description Gateway FACTURACION (HSRP)
 encapsulation dot1Q 46
 ip address 172.16.26.67 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_FACT_IN in
 standby 46 ip 172.16.26.65
 standby 46 priority 110
 standby 46 preempt
 standby 46 track GigabitEthernet0/0 30

router ospf 1
 router-id 2.2.1.3
 network 192.168.16.68 0.0.0.3 area 0
 network 172.16.26.0 0.0.0.63 area 0
 network 172.16.26.64 0.0.0.63 area 0
 passive-interface GigabitEthernet0/1.36
 passive-interface GigabitEthernet0/1.46
end
write
```

#### Router 2911 - RD1-ISP2

```bash
enable
configure terminal
hostname RD1-ISP2
no ip domain-lookup

interface GigabitEthernet0/0
 description Segmento core (SW1-ISP2)
 ip address 172.16.26.131 255.255.255.240
 no shutdown

interface GigabitEthernet0/1
 description Enlace a RD2-ISP2
 ip address 192.168.16.73 255.255.255.252
 no shutdown

interface GigabitEthernet0/2
 description Enlace a RD3-ISP2
 ip address 192.168.16.77 255.255.255.252
 no shutdown

router ospf 1
 router-id 2.2.2.1
 network 172.16.26.128 0.0.0.15 area 0
 network 192.168.16.72 0.0.0.3 area 0
 network 192.168.16.76 0.0.0.3 area 0
end
write
```

#### Router 2911 - RD2-ISP2

```bash
enable
configure terminal
hostname RD2-ISP2
no ip domain-lookup

ip access-list extended ACL_VENTAS_IN
 permit icmp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63
 permit tcp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63 eq 80
 permit tcp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63 established
 deny ip 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63
 permit icmp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127
 permit tcp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127 eq 80
 permit tcp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127 established
 deny ip 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127
 permit ip any any

ip access-list extended ACL_FACT_IN
 permit icmp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63
 permit tcp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63 established
 deny ip 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63
 deny ip 172.16.26.64 0.0.0.63 172.16.16.128 0.0.0.127
 permit ip any any
exit

interface GigabitEthernet0/0
 description Enlace a RD1-ISP2
 ip address 192.168.16.74 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Trunk a SW5-ISP2
 no ip address
 no shutdown

interface GigabitEthernet0/1.36
 description Gateway VENTAS (HSRP)
 encapsulation dot1Q 36
 ip address 172.16.26.4 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_VENTAS_IN in
 standby 36 ip 172.16.26.1
 standby 36 priority 100
 standby 36 preempt
 standby 36 track GigabitEthernet0/0 30

interface GigabitEthernet0/1.46
 description Gateway FACTURACION (HSRP)
 encapsulation dot1Q 46
 ip address 172.16.26.68 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_FACT_IN in
 standby 46 ip 172.16.26.65
 standby 46 priority 100
 standby 46 preempt
 standby 46 track GigabitEthernet0/0 30

router ospf 1
 router-id 2.2.2.2
 network 192.168.16.72 0.0.0.3 area 0
 network 172.16.26.0 0.0.0.63 area 0
 network 172.16.26.64 0.0.0.63 area 0
 passive-interface GigabitEthernet0/1.36
 passive-interface GigabitEthernet0/1.46
end
write
```

#### Router 2911 - RD3-ISP2

```bash
enable
configure terminal
hostname RD3-ISP2
no ip domain-lookup

ip access-list extended ACL_VENTAS_IN
 permit icmp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63
 permit tcp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63 eq 80
 permit tcp 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63 established
 deny ip 172.16.26.0 0.0.0.63 172.16.26.64 0.0.0.63
 permit icmp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127
 permit tcp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127 eq 80
 permit tcp 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127 established
 deny ip 172.16.26.0 0.0.0.63 172.16.16.128 0.0.0.127
 permit ip any any

ip access-list extended ACL_FACT_IN
 permit icmp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63
 permit tcp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63 eq 80
 permit tcp 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63 established
 deny ip 172.16.26.64 0.0.0.63 172.16.26.0 0.0.0.63
 deny ip 172.16.26.64 0.0.0.63 172.16.16.128 0.0.0.127
 permit ip any any
exit

interface GigabitEthernet0/0
 description Enlace a RD1-ISP2
 ip address 192.168.16.78 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Trunk a SW5-ISP2
 no ip address
 no shutdown

interface GigabitEthernet0/1.36
 description Gateway VENTAS (HSRP)
 encapsulation dot1Q 36
 ip address 172.16.26.5 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_VENTAS_IN in
 standby 36 ip 172.16.26.1
 standby 36 priority 90
 standby 36 preempt
 standby 36 track GigabitEthernet0/0 30

interface GigabitEthernet0/1.46
 description Gateway FACTURACION (HSRP)
 encapsulation dot1Q 46
 ip address 172.16.26.69 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_FACT_IN in
 standby 46 ip 172.16.26.65
 standby 46 priority 90
 standby 46 preempt
 standby 46 track GigabitEthernet0/0 30

router ospf 1
 router-id 2.2.2.3
 network 192.168.16.76 0.0.0.3 area 0
 network 172.16.26.0 0.0.0.63 area 0
 network 172.16.26.64 0.0.0.63 area 0
 passive-interface GigabitEthernet0/1.36
 passive-interface GigabitEthernet0/1.46
end
write
```

#### Switch 2960 - SW2-ISP2

```bash
enable
configure terminal
hostname SW2-ISP2
no ip domain-lookup

vlan 36
 name VLAN_VENTAS
vlan 46
 name VLAN_FACTURACION
exit

interface range FastEthernet0/1 - 2
 description LACP a SW3-ISP2
 switchport mode trunk
 channel-group 1 mode active
exit

interface range FastEthernet0/3 - 4
 description LACP a SW4-ISP2
 switchport mode trunk
 channel-group 2 mode active
exit

interface Port-channel1
 switchport mode trunk
interface Port-channel2
 switchport mode trunk

interface GigabitEthernet0/1
 description Trunk a RI2-ISP2
 switchport mode trunk

interface GigabitEthernet0/2
 description Trunk a RI3-ISP2
 switchport mode trunk

interface FastEthernet0/5
 description Trunk L2 hacia SW5-ISP2 (VLAN 36 y 46 en ambas mitades)
 switchport mode trunk
end
write
```

#### Switch 2960 - SW3-ISP2

```bash
enable
configure terminal
hostname SW3-ISP2
no ip domain-lookup

vlan 36
 name VLAN_VENTAS
vlan 46
 name VLAN_FACTURACION
exit

interface range FastEthernet0/1 - 2
 description LACP a SW2-ISP2
 switchport mode trunk
 channel-group 1 mode passive
exit

interface Port-channel1
 switchport mode trunk

interface FastEthernet0/3
 description Laptop8 (Rojo - Ventas)
 switchport mode access
 switchport access vlan 36
 spanning-tree portfast

interface FastEthernet0/4
 description PC8 (Azul - Facturacion)
 switchport mode access
 switchport access vlan 46
 spanning-tree portfast
end
write
```

#### Switch 2960 - SW4-ISP2

```bash
enable
configure terminal
hostname SW4-ISP2
no ip domain-lookup

vlan 36
 name VLAN_VENTAS
vlan 46
 name VLAN_FACTURACION
exit

interface range FastEthernet0/1 - 2
 description LACP a SW2-ISP2
 switchport mode trunk
 channel-group 2 mode passive
exit

interface Port-channel2
 switchport mode trunk

interface FastEthernet0/3
 description PC9 (Azul - Facturacion)
 switchport mode access
 switchport access vlan 46
 spanning-tree portfast

interface FastEthernet0/4
 description Laptop9 (Rojo - Ventas)
 switchport mode access
 switchport access vlan 36
 spanning-tree portfast
end
write
```

#### Switch 2960 - SW5-ISP2

```bash
enable
configure terminal
hostname SW5-ISP2
no ip domain-lookup

vlan 36
 name VLAN_VENTAS
vlan 46
 name VLAN_FACTURACION
exit

interface range FastEthernet0/1 - 2
 description LACP a SW6-ISP2
 switchport mode trunk
 channel-group 1 mode active
exit

interface range FastEthernet0/3 - 4
 description LACP a SW7-ISP2
 switchport mode trunk
 channel-group 2 mode active
exit

interface Port-channel1
 switchport mode trunk
interface Port-channel2
 switchport mode trunk

interface GigabitEthernet0/1
 description Trunk a RD2-ISP2
 switchport mode trunk

interface GigabitEthernet0/2
 description Trunk a RD3-ISP2
 switchport mode trunk

interface FastEthernet0/5
 description Trunk L2 hacia SW2-ISP2 (VLAN 36 y 46 en ambas mitades)
 switchport mode trunk
end
write
```

#### Switch 2960 - SW6-ISP2

```bash
enable
configure terminal
hostname SW6-ISP2
no ip domain-lookup

vlan 36
 name VLAN_VENTAS
vlan 46
 name VLAN_FACTURACION
exit

interface range FastEthernet0/1 - 2
 description LACP a SW5-ISP2
 switchport mode trunk
 channel-group 1 mode passive
exit

interface Port-channel1
 switchport mode trunk

interface FastEthernet0/3
 description PC10 (Rojo - Ventas)
 switchport mode access
 switchport access vlan 36
 spanning-tree portfast

interface FastEthernet0/4
 description Laptop10 (Rojo - Ventas)
 switchport mode access
 switchport access vlan 36
 spanning-tree portfast
end
write
```

#### Switch 2960 - SW7-ISP2

```bash
enable
configure terminal
hostname SW7-ISP2
no ip domain-lookup

vlan 36
 name VLAN_VENTAS
vlan 46
 name VLAN_FACTURACION
exit

interface range FastEthernet0/1 - 2
 description LACP a SW5-ISP2
 switchport mode trunk
 channel-group 2 mode passive
exit

interface Port-channel2
 switchport mode trunk

interface FastEthernet0/3
 description PC11 (Rojo - Ventas)
 switchport mode access
 switchport access vlan 36
 spanning-tree portfast

interface FastEthernet0/4
 description Laptop11 (Azul - Facturacion)
 switchport mode access
 switchport access vlan 46
 spanning-tree portfast
end
write
```

#### Server DHCP

IP estática del servidor:

- IP: `172.16.26.140`
- Máscara: `255.255.255.240`
- Gateway: `172.16.26.129`
- DNS Server: `172.16.16.10`

#### 4.7.1 DHCP

Pools DHCP:

| Pool               | Gateway         | DNS            | IP inicial      | Máscara           | Máx. |
| ------------------ | --------------- | -------------- | --------------- | ----------------- | ---- |
| VLAN16_ADMIN       | `172.16.16.1`   | `172.16.16.10` | `172.16.16.11`  | `255.255.255.128` | 110  |
| VLAN26_CLIENTE     | `172.16.16.129` | `172.16.16.10` | `172.16.16.130` | `255.255.255.128` | 110  |
| VLAN36_VENTAS      | `172.16.26.1`   | `172.16.16.10` | `172.16.26.10`  | `255.255.255.192` | 50   |
| VLAN46_FACTURACION | `172.16.26.65`  | `172.16.16.10` | `172.16.26.75`  | `255.255.255.192` | 50   |
| VLAN56_SOPORTE     | `172.16.32.1`   | `172.16.16.10` | `172.16.32.10`  | `255.255.255.192` | 50   |
| VLAN66_SEGURIDAD   | `172.16.32.65`  | `172.16.16.10` | `172.16.32.75`  | `255.255.255.192` | 50   |

### 3.3 ISP 3: MoonLink

#### Multilayer Switch 3650 - MS3

```bash
enable
configure terminal
hostname MS3
no ip domain-lookup
ip routing

! Enlaces inter-ISP (fibra)
interface GigabitEthernet1/1/2
 description BGP a MS1 (FibraNet GT)
 no switchport
 ip address 192.168.16.10 255.255.255.252
 no shutdown

interface GigabitEthernet1/1/1
 description BGP a MS2 (AndinaCom)
 no switchport
 ip address 192.168.16.6 255.255.255.252
 no shutdown

! Enlace interno hacia el hub R1-ISP3
interface GigabitEthernet1/0/1
 description Enlace a R1-ISP3 (Hub)
 no switchport
 ip address 192.168.16.129 255.255.255.252
 no shutdown

router eigrp 100
 eigrp router-id 3.3.0.1
 network 192.168.16.128 0.0.0.3
 passive-interface GigabitEthernet1/1/1
 passive-interface GigabitEthernet1/1/2
 redistribute bgp 300 metric 1000000 10 255 1 1500
 no auto-summary

router bgp 300
 bgp router-id 3.3.0.1
 bgp log-neighbor-changes
 neighbor 192.168.16.9 remote-as 100
 neighbor 192.168.16.5 remote-as 200
 network 172.16.32.0 mask 255.255.255.192
 network 172.16.32.64 mask 255.255.255.192
end
write
```

#### Router 2911 - R1-ISP3

```bash
enable
configure terminal
hostname R1-ISP3
no ip domain-lookup

interface GigabitEthernet0/0
 description Hacia MS3 (Core BGP)
 ip address 192.168.16.130 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Spoke hacia R3-ISP3 (Seguridad)
 ip address 192.168.16.133 255.255.255.252
 no shutdown

interface GigabitEthernet0/2
 description Spoke hacia R5-ISP3 (Soporte)
 ip address 192.168.16.137 255.255.255.252
 no shutdown

router eigrp 100
 eigrp router-id 3.3.1.1
 network 192.168.16.128 0.0.0.3
 network 192.168.16.132 0.0.0.3
 network 192.168.16.136 0.0.0.3
 no auto-summary
end
write
```

#### Router WRT300N - R2-ISP3

- LAN: IP: `172.16.32.5` / Máscara: `255.255.255.192`
- Wireless: SSID: `MoonLink_Soporte` / Authentication: `WPA2-PSK` / PSK Pass Phrase: `moonlinkr2` / SSID: `Disabled`
- Contraseña: `Diego365`

#### Router 2911 - R3-ISP3

```bash
enable
configure terminal
hostname R3-ISP3
no ip domain-lookup

ip access-list extended ACL_SEG_IN
 permit udp any any eq 1985
 permit udp any eq bootpc any eq bootps
 permit icmp 172.16.32.64 0.0.0.63 any
 permit tcp 172.16.32.64 0.0.0.63 any eq 80
 permit tcp 172.16.32.64 0.0.0.63 any eq 443
 permit udp 172.16.32.64 0.0.0.63 any eq domain
 deny ip any any

ip access-list extended ACL_SEG_OUT
 permit icmp any 172.16.32.64 0.0.0.63 echo-reply
 permit tcp any 172.16.32.64 0.0.0.63 established
 permit udp any eq domain 172.16.32.64 0.0.0.63
 permit udp any eq bootps any eq bootpc
 deny ip any any
exit

interface GigabitEthernet0/0
 description Hacia R1-ISP3 (Hub)
 ip address 192.168.16.134 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Trunk hacia SW1-R3 (VLAN 56 y 66)
 no ip address
 no shutdown

interface GigabitEthernet0/1.56
 description Gateway SOPORTE (HSRP)
 encapsulation dot1Q 56
 ip address 172.16.32.2 255.255.255.192
 ip helper-address 172.16.26.140
 standby 56 ip 172.16.32.1
 standby 56 priority 110
 standby 56 preempt
 standby 56 track GigabitEthernet0/0 30

interface GigabitEthernet0/1.66
 description Gateway SEGURIDAD (HSRP)
 encapsulation dot1Q 66
 ip address 172.16.32.66 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_SEG_IN in
 ip access-group ACL_SEG_OUT out
 standby 66 ip 172.16.32.65
 standby 66 priority 110
 standby 66 preempt
 standby 66 track GigabitEthernet0/0 30

router eigrp 100
 eigrp router-id 3.3.3.1
 network 192.168.16.132 0.0.0.3
 network 172.16.32.0 0.0.0.63
 network 172.16.32.64 0.0.0.63
 passive-interface GigabitEthernet0/1.56
 passive-interface GigabitEthernet0/1.66
 no auto-summary
end
write
```

#### Router WRT300N - R4-ISP3

- LAN: IP: `172.16.32.68` / Máscara: `255.255.255.192`
- Wireless: SSID: `MoonLink_Seguridad` / Authentication: `WPA2-PSK` / PSK Pass Phrase: `moonlinkr4` / SSID: `Disabled`
- Contraseña: `Diego365`

#### Router 2911 - R5-ISP3

```bash
enable
configure terminal
hostname R5-ISP3
no ip domain-lookup

ip access-list extended ACL_SEG_IN
 permit udp any any eq 1985
 permit udp any eq bootpc any eq bootps
 permit icmp 172.16.32.64 0.0.0.63 any
 permit tcp 172.16.32.64 0.0.0.63 any eq 80
 permit tcp 172.16.32.64 0.0.0.63 any eq 443
 permit udp 172.16.32.64 0.0.0.63 any eq domain
 deny ip any any

ip access-list extended ACL_SEG_OUT
 permit icmp any 172.16.32.64 0.0.0.63 echo-reply
 permit tcp any 172.16.32.64 0.0.0.63 established
 permit udp any eq domain 172.16.32.64 0.0.0.63
 permit udp any eq bootps any eq bootpc
 deny ip any any
exit

interface GigabitEthernet0/0
 description Hacia R1-ISP3 (Hub)
 ip address 192.168.16.138 255.255.255.252
 no shutdown

interface GigabitEthernet0/1
 description Trunk hacia SW1-R5 (VLAN 56 y 66)
 no ip address
 no shutdown

interface GigabitEthernet0/1.56
 description Gateway SOPORTE (HSRP)
 encapsulation dot1Q 56
 ip address 172.16.32.3 255.255.255.192
 ip helper-address 172.16.26.140
 standby 56 ip 172.16.32.1
 standby 56 priority 100
 standby 56 preempt
 standby 56 track GigabitEthernet0/0 30

interface GigabitEthernet0/1.66
 description Gateway SEGURIDAD (HSRP)
 encapsulation dot1Q 66
 ip address 172.16.32.67 255.255.255.192
 ip helper-address 172.16.26.140
 ip access-group ACL_SEG_IN in
 ip access-group ACL_SEG_OUT out
 standby 66 ip 172.16.32.65
 standby 66 priority 100
 standby 66 preempt
 standby 66 track GigabitEthernet0/0 30

router eigrp 100
 eigrp router-id 3.3.5.1
 network 192.168.16.136 0.0.0.3
 network 172.16.32.0 0.0.0.63
 network 172.16.32.64 0.0.0.63
 passive-interface GigabitEthernet0/1.56
 passive-interface GigabitEthernet0/1.66
 no auto-summary
end
write
```

#### Switch 2960 - SW1-R3

```bash
enable
configure terminal
hostname SW1-R3
no ip domain-lookup

vlan 56
 name VLAN_SOPORTE
vlan 66
 name VLAN_SEGURIDAD
exit

interface GigabitEthernet0/1
 description Trunk hacia R3 Gi0/1
 switchport mode trunk

interface FastEthernet0/3
 description Hacia WRT R4 (AP Wi-Fi Seguridad)
 switchport mode access
 switchport access vlan 66

interface FastEthernet0/4
 description Trunk hacia SW1-R5 Fa0/4
 switchport mode trunk

interface range FastEthernet0/1 - 2
 description LACP hacia SW2-R3
 switchport mode trunk
 channel-group 1 mode active
exit

interface Port-channel1
 switchport mode trunk
end
write
```

#### Switch 2960 - SW2-R3

```bash
enable
configure terminal
hostname SW2-R3
no ip domain-lookup

vlan 56
 name VLAN_SOPORTE
vlan 66
 name VLAN_SEGURIDAD
exit

interface range FastEthernet0/3 - 4
 description LACP hacia SW1-R3
 switchport mode trunk
 channel-group 1 mode passive
exit

interface Port-channel1
 switchport mode trunk

interface FastEthernet0/1
 description Laptop1 (Morado - Seguridad)
 switchport mode access
 switchport access vlan 66
 spanning-tree portfast

interface FastEthernet0/2
 description PC1 (Naranja - Soporte)
 switchport mode access
 switchport access vlan 56
 spanning-tree portfast
end
write
```

#### Switch 2960 - SW1-R5

```bash
enable
configure terminal
hostname SW1-R5
no ip domain-lookup

vlan 56
 name VLAN_SOPORTE
vlan 66
 name VLAN_SEGURIDAD
exit

interface GigabitEthernet0/1
 description Trunk hacia R5 Gi0/1
 switchport mode trunk

interface FastEthernet0/3
 description Hacia WRT R2 (AP Wi-Fi Soporte)
 switchport mode access
 switchport access vlan 56

interface FastEthernet0/4
 description Trunk hacia SW1-R3 Fa0/4
 switchport mode trunk

interface range FastEthernet0/1 - 2
 description LACP hacia SW2-R5
 switchport mode trunk
 channel-group 2 mode active
exit

interface Port-channel2
 switchport mode trunk
end
write
```

#### Switch 2960 - SW2-R5

```bash
enable
configure terminal
hostname SW2-R5
no ip domain-lookup

vlan 56
 name VLAN_SOPORTE
vlan 66
 name VLAN_SEGURIDAD
exit

interface range FastEthernet0/3 - 4
 description LACP hacia SW1-R5
 switchport mode trunk
 channel-group 2 mode passive
exit

interface Port-channel2
 switchport mode trunk

interface FastEthernet0/1
 description PC3 (Naranja - Soporte)
 switchport mode access
 switchport access vlan 56
 spanning-tree portfast

interface FastEthernet0/2
 description Laptop3 (Naranja - Soporte)
 switchport mode access
 switchport access vlan 56
 spanning-tree portfast
end
write
```

## 4. Pruebas

### 4.1 Seguridad - Cualquier depto

![prueba1](./imgs/prueba1.png)

### 4.2 Cualquier depto - Seguridad

![prueba2](./imgs/prueba2.png)

### 4.3 Soporte - Cualquier depto

![prueba3.1](./imgs/prueba3.1.png)

![prueba3.2](./imgs/prueba3.2.png)

### 4.4 Administración - Cualquier depto

![prueba4.1](./imgs/prueba4.1.png)

![prueba4.2](./imgs/prueba4.2.png)

### 4.5 Atención al cliente - Ventas

![prueba5](./imgs/prueba5.png)

### 4.6 Atención al cliente - Facturación

![prueba6](./imgs/prueba6.png)

### 4.7 Facturación - Ventas

![prueba7](./imgs/prueba7.png)

### 4.8 Facturación - Atención al cliente

![prueba8](./imgs/prueba8.png)

### 4.9 Ventas - Facturación y Atención al cliente

![prueba9.1](./imgs/prueba9.1.png)

![prueba9.2](./imgs/prueba9.2.png)

### 4.10 Pruebas con comandos

![comando1](./imgs/comando1.png)

![comando2.1](./imgs/comando2.1.png)

![comando2.2](./imgs/comando2.2.png)

![comando2.3](./imgs/comando2.3.png)

![comando2.4](./imgs/comando2.4.png)

![comando3](./imgs/comando3.png)

![comnado4.1](./imgs/comando4.1.png)

![comando4.2](./imgs/comando4.2.png)

![comando4.3](./imgs/comando4.3.png)

![comando5.1](./imgs/comando5.1.png)

![comando5.2](./imgs/comando5.2.png)

![comando5.3](./imgs/comando5.3.png)

![comando5.4](./imgs/comando5.4.png)

![comando5.5](./imgs/comando5.5.png)

![comando6.1](./imgs/comando6.1.png)

![comando6.2](./imgs/comando6.2.png)

![comando6.3](./imgs/comando6.3.png)

## 5. Comandos utilizados

| Comando                                                          | Propósito                                                                                                               |
| ---------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `show vlan brief`                                                | Ver las VLAN creadas y los puertos asignados a cada VLAN.                                                               |
| `show interfaces trunk`                                          | Verificar los enlaces trunk y las VLAN que están en `forwarding`.                                                       |
| `show etherchannel summary`                                      | Verificar el estado de los EtherChannels/LACP y sus puertos miembros.                                                   |
| `show ip ospf neighbor`                                          | Verificar los vecinos OSPF y comprobar que las adyacencias estén establecidas.                                          |
| `show ip route ospf`                                             | Mostrar únicamente las rutas aprendidas mediante OSPF.                                                                  |
| `show standby brief`                                             | Verificar el estado de HSRP, incluyendo routers `Active`, `Standby`, `Listen` y sus prioridades.                        |
| `show ip interface brief`                                        | Verificar el estado (`up/down`) y las direcciones IP de las interfaces.                                                 |
| `show ip eigrp neighbors`                                        | Verificar los vecinos EIGRP y sus adyacencias.                                                                          |
| `show ip bgp summary`                                            | Consultar el estado de las sesiones BGP y los prefijos recibidos.                                                       |
| `show ip bgp`                                                    | Mostrar la tabla BGP y los prefijos aprendidos/anunciados.                                                              |
| `ipconfig /release`                                              | Liberar la configuración IP obtenida por DHCP en un equipo.                                                             |
| `ipconfig /renew`                                                | Solicitar nuevamente una configuración IP mediante DHCP.                                                                |
| `ipconfig /all`                                                  | Mostrar detalladamente la configuración IP, gateway, DHCP, DNS y otros parámetros de red.                               |
| `ping`                                                           | Comprobar la conectividad entre dispositivos.                                                                           |
| `ping www.proyecto2_202307716.com`                               | Comprobar conectividad y verificar que el nombre del servidor sea resuelto por DNS.                                     |
| `ping -t 172.16.16.10`                                           | Mantener un ping continuo al servidor para observar interrupciones y recuperación durante las pruebas de redundancia.   |
| `tracert 172.16.16.10`                                           | Mostrar la ruta o los saltos utilizados para llegar al servidor y comprobar qué router HSRP está actuando como gateway. |
| `show access-lists`                                              | Mostrar las ACL configuradas y sus contadores de coincidencias (`matches`).                                             |
| `show ip interface GigabitEthernet0/2.26 \| include access list` | Verificar qué ACL está aplicada sobre la subinterfaz `Gi0/2.26`.                                                        |
| `show ip interface GigabitEthernet0/1.66 \| include access list` | Verificar qué ACL está aplicada sobre la subinterfaz `Gi0/1.66`.                                                        |
