# 📘 Guía de Comandos Especiales para Configuración de Routers
_Actualizado, organizado y explicado para técnicos en acción_

---

## 🧠 Índice

- [📘 Guía de Comandos Especiales para Configuración de Routers](#-guía-de-comandos-especiales-para-configuración-de-routers)
  - [🧠 Índice](#-índice)
  - [Cisco](#cisco)
    - [✅ Verificación y diagnóstico](#-verificación-y-diagnóstico)
  - [🔁 Reseteo y recuperación](#-reseteo-y-recuperación)
    - [Borrar configuración completa:](#borrar-configuración-completa)
  - [💾 IOS: verificación, carga y descarga](#-ios-verificación-carga-y-descarga)
    - [Verificación de imagen desde USB:](#verificación-de-imagen-desde-usb)
    - [Cargar IOS desde TFTP (modo ROMMON):](#cargar-ios-desde-tftp-modo-rommon)
    - [Subir IOS al router desde TFTP:](#subir-ios-al-router-desde-tftp)
    - [Descargar IOS desde router a TFTP:](#descargar-ios-desde-router-a-tftp)
- [⚙️ Configuración general](#️-configuración-general)
- [📊 Análisis de red y saturación](#-análisis-de-red-y-saturación)
    - [Activar top-talkers (NetFlow):](#activar-top-talkers-netflow)
    - [Habilitar cache por interfaz:](#habilitar-cache-por-interfaz)
    - [Ver flujo de tráfico actual:](#ver-flujo-de-tráfico-actual)
  - [🧱 VLAN y PortFast](#-vlan-y-portfast)
- [Huawei](#huawei)
    - [🔍 Verificación de transceivers](#-verificación-de-transceivers)
    - [🔄 Reseteo y administración](#-reseteo-y-administración)
    - [Ping de prueba grande (MTU):](#ping-de-prueba-grande-mtu)
  - [🧰 Configuración básica](#-configuración-básica)
- [Juniper](#juniper)
    - [🧱 Configuración inicial](#-configuración-inicial)
  - [📈 Comandos de diagnóstico](#-comandos-de-diagnóstico)
  - [🧩 Carga de configuración](#-carga-de-configuración)
  - [🔁 Reinicio y recuperación](#-reinicio-y-recuperación)
  - [iPerf3 - Pruebas de rendimiento](#iperf3---pruebas-de-rendimiento)
    - [Ejemplos por región:](#ejemplos-por-región)
- [🔐 Passwords y credenciales comunes](#-passwords-y-credenciales-comunes)

---

## Cisco

### ✅ Verificación y diagnóstico

```bash
show version
# Muestra detalles del hardware, imagen IOS, memoria y tiempo encendido.

show ip route
# Tabla de enrutamiento en uso (rutas estáticas y dinámicas).

show ip interface brief
# Resumen del estado y dirección IP de las interfaces.

show interfaces
# Estadísticas completas: velocidad, duplex, errores, paquetes, etc.

show cdp neighbors
# Lista de dispositivos Cisco directamente conectados.

show flash
# Archivos almacenados en la memoria flash (como IOS).

show ip traffic
# Tráfico IP total por protocolos.

show processes cpu history
# Carga del CPU a lo largo del tiempo (gráfico por consola).

show vlan brief
# Listado de VLANs configuradas (solo en switches).

clear counters
# Reinicia los contadores de paquetes por interfaz.
```

##  🔁 Reseteo y recuperación

```bash
rommon1> confreg 0x2142
# Ignora la configuración guardada en el arranque.

rommon2> reset
# Reinicia el equipo.

Router> enable
# Entra en modo privilegiado.

Router# write memory
# Guarda la configuración vacía actual (opcional).

Router# configure terminal
Router(config)# config-register 0x2102
# Restaura el registro para que cargue la startup-config normalmente.

Router# reload
# Reinicia el router con la configuración recuperada.
```
### Borrar configuración completa:

```bash
erase startup-config
reload
```
## 💾 IOS: verificación, carga y descarga
### Verificación de imagen desde USB:

```bash
copy usb0:archivo.bin bootflash:
verify /md5 flash:archivo.bin
boot system flash archivo.bin
do write
reload
```
### Cargar IOS desde TFTP (modo ROMMON):

```bash
rommon 1 > IP_ADDRESS=192.168.1.1
rommon 2 > IP_SUBNET_MASK=255.255.255.0
rommon 3 > DEFAULT_GATEWAY=192.168.1.2
rommon 4 > TFTP_SERVER=192.168.1.2
rommon 5 > TFTP_FILE=nombre.bin
rommon 6 > tftpdnld
```
### Subir IOS al router desde TFTP:

```bash
copy tftp: flash:
```
### Descargar IOS desde router a TFTP:

```bash
copy flash: tftp:
```
# ⚙️ Configuración general

```bash
show running-config
# Config actual en ejecución (RAM).

show startup-config
# Config guardada en memoria NVRAM.

configure terminal
# Entra al modo de configuración.

interface FastEthernet0/0
 description LINK
 ip address 192.168.1.1 255.255.255.0
 no shutdown
```
# 📊 Análisis de red y saturación
### Activar top-talkers (NetFlow):
```bash
ip flow-top-talkers
sort-by bytes
top 10
```
### Habilitar cache por interfaz:
```bash
interface FastEthernet4
ip route-cache flow
```
### Ver flujo de tráfico actual:
```bash
show ip cache flow
show ip flow top-talkers
```
## 🧱 VLAN y PortFast
```bash
vlan 10
name DEPARTAMENTO
!
interface FastEthernet0/1
spanning-tree portfast
# PortFast acelera el inicio de puertos (solo para hosts, no switches).
```
# Huawei
### 🔍 Verificación de transceivers
```bash
display transceiver interface GigabitEthernet 0/0/8 verbose
# Verifica niveles ópticos, potencia TX/RX.
```
### 🔄 Reseteo y administración
```bash
reset saved-configuration
# Borra configuración guardada.

reboot fast
# Reinicia inmediatamente.
```
### Ping de prueba grande (MTU):
```bash
ping -c 30 -s 1472 -M 1 [ip_siguiente_salto]
```
## 🧰 Configuración básica
```bash
display current-configuration
# Muestra la configuración en ejecución.

display ip interface brief
# Muestra estado e IPs asignadas.

ip netstream timeout active 1
ip netstream timeout inactive 20
ip netstream export version 9
# Configuración Netstream (equivalente a NetFlow).
```
# Juniper
### 🧱 Configuración inicial
```bash
delete
set system host-name MiRouter
set system root-authentication plain-text-password
commit check
commit
```
## 📈 Comandos de diagnóstico
```bash
run show version
run show configuration
run show interface terse
run show interfaces extensive
run show system alarms
run show log messages
run show security flow session
run show chassis hardware
# Información detallada para ver hardware, interfaces y tráfico.
```
## 🧩 Carga de configuración
```bash
load override terminal
# Pega configuración completa (modo brackets).

CTRL+D
# Finaliza pegado de config.

commit
# Guarda los cambios.
```
## 🔁 Reinicio y recuperación

```bash
request system reboot
request system power-off
request system configuration rescue save
request system configuration rescue delete
```
## iPerf3 - Pruebas de rendimiento
```bash
iperf3 -c [host] -t 1000 -i 10 -P 18 -R
# Cliente conecta al servidor especificado durante 1000s, reporta cada 10s,
# lanza 18 hilos en modo reverso (el servidor envía tráfico).
```
### Ejemplos por región:
```bash
# Francia
iperf3 -c bouygues.iperf.fr -p 9200 -t 10000 -i 10 -P 18 -R

# Holanda
iperf3 -c speedtest.serverius.net -p 5002 -t 1000 -i 10 -P 18 -R

# Asia
iperf3 -c iperf.it-north.net -p 5200 -t 1000 -i 10 -P 18 -R

# EE.UU
iperf3 -c iperf.he.net -p 5201 -t 1000 -i 10 -P 18 -R
```
# 🔐 Passwords y credenciales comunes
| Dispositivo | Usuario | Contraseña                 |
| ----------- | ------- | -------------------------- |
| Juniper     | NOC     | `1_#tZytu5H`               |
| Cisco       | —       | `scylla&//`, `val&kyry`    |
| Huawei      | admin   | `Admin@huawei`, `Danzig69` |

