# Documentación Técnica Proyecto 1 - RC2 - 202307716

## 1. Topología Completa

## 2. Cálculos de Subnetting (VLSM/FLSM)

### 2.1 VLSM

**Calculadora utilizada para realizar los cálculos de VLSM:**
[Calculadora VLSM](https://www.arcadio.gq/calculadora-subredes-vlsm.html)

![VLSM](./imgs/vlsm.png)

#### 2.1.1 IP de Red = 192.188.16.0 /24

| **VLAN**                           | **ID VLAN** | **No. de Hosts** | **IP de Red**     | **Máscara**     | **Wildcard** | **Primer Host** | **Último Host** | **Broadcast** |
| :--------------------------------- | :---------- | :--------------- | :---------------- | :-------------- | :----------- | :-------------- | :-------------- | :------------ |
| VLAN_Naranja_EdificioIZQ_202307716 | 16          | 6                | 192.188.16.0 /29  | 255.255.255.248 | 0.0.0.7      | 192.188.16.1    | 192.188.16.6    | 192.188.16.7  |
| VLAN_Verde_EdificioIZQ_202307716   | 26          | 6                | 192.188.16.8 /29  | 255.255.255.248 | 0.0.0.7      | 192.188.16.9    | 192.188.16.14   | 192.188.16.15 |
| VLAN_Naranja_EdificioDER_202307716 | 36          | 6                | 192.188.16.16 /29 | 255.255.255.248 | 0.0.0.7      | 192.188.16.17   | 192.188.16.22   | 192.188.16.23 |
| VLAN_Verde_EdificioDER_202307716   | 46          | 6                | 192.188.16.24 /29 | 255.255.255.248 | 0.0.0.7      | 192.188.16.25   | 192.188.16.30   | 192.188.16.31 |
| VLAN_ADMIN_EdificioADMIN_202307716 | 56          | 6                | 192.188.16.32 /29 | 255.255.255.248 | 0.0.0.7      | 192.188.16.33   | 192.188.16.38   | 192.188.16.39 |

### 2.2 FLSM

**Calculadora utilizada para realizar los cálculos de FLSM:**
[Calculadora VLSM](https://www.arcadio.gq/calculadora-subredes-flsm.html)

![FLSM](./imgs/flsm0.png)

![FLSM1](./imgs/flsm1.png)

![FLSM2](./imgs/flsm2.png)

![FLSM3](./imgs/flsm3.png)

![FLSM4](./imgs/flsm4.png)

#### 2.2.1 Tabla de FLSM

| **IP de Red**  | **Máscara**     | **Wildcard** | **Primer Host** | **Último Host** | **IP Broadcast** |
| :------------- | :-------------- | :----------- | :-------------- | :-------------- | :--------------- |
| 10.4.16.0 /30  | 255.255.255.252 | 0.0.0.3      | 10.4.16.1       | 10.4.16.2       | 10.4.16.3        |
| 10.4.16.4 /30  | 255.255.255.252 | 0.0.0.3      | 10.4.16.5       | 10.4.16.6       | 10.4.16.7        |
| 10.4.16.8 /30  | 255.255.255.252 | 0.0.0.3      | 10.4.16.9       | 10.4.16.10      | 10.4.16.11       |
| 10.4.16.12 /30 | 255.255.255.252 | 0.0.0.3      | 10.4.16.13      | 10.4.16.14      | 10.4.16.15       |
| 10.4.16.16 /30 | 255.255.255.252 | 0.0.0.3      | 10.4.16.17      | 10.4.16.18      | 10.4.16.19       |
