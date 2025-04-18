# 🛠️ Manual Avanzado CLI - Issabel PBX / Asterisk (Navaja Suiza de Comandos)
**Autor:** Jesús Flores – Soporte TI  
**Versión:** 2025.04  
**Propósito:** Guía profesional de referencia para administración, monitoreo y solución de problemas en entornos Asterisk e Issabel desde CLI.

---

## 📑 ÍNDICE

- [1. 🧭 Navegación y ayuda CLI](#1--navegación-y-ayuda-cli)
- [2. 📞 Extensiones PJSIP y SIP](#2--extensiones-pjsip-y-sip)
    - [🔹 PJSIP](#-pjsip)
    - [🔹 SIP clásico (chan\_sip)](#-sip-clásico-chan_sip)
    - [🔹 IAX2](#-iax2)
- [3. 📡 Troncales y registros externos](#3--troncales-y-registros-externos)
- [4. 📊 Canales y llamadas](#4--canales-y-llamadas)
- [5. 🔁 Recargas, módulos y reinicios](#5--recargas-módulos-y-reinicios)
- [6. 📥 Colas y agentes](#6--colas-y-agentes)
- [7. 🎙️ Conferencias](#7-️-conferencias)
- [8. 🔊 Audio, RTP y Codecs](#8--audio-rtp-y-codecs)
- [9. 🧠 ASTDB (Base de datos interna)](#9--astdb-base-de-datos-interna)
- [10. 🔐 Seguridad, AMI y logs](#10--seguridad-ami-y-logs)
- [11. 🛠️ Extras y herramientas](#11-️-extras-y-herramientas)
- [12. 🔑 Cambio de contraseñas](#12--cambio-de-contraseñas)
    - [🔹 Extensión PJSIP](#-extensión-pjsip)
    - [🔹 Extensión SIP clásico](#-extensión-sip-clásico)
    - [🔹 Usuario AMI (manager.conf)](#-usuario-ami-managerconf)
    - [🔹 Usuario del sistema Linux](#-usuario-del-sistema-linux)
- [13. 🧩 Script de diagnóstico rápido](#13--script-de-diagnóstico-rápido)
- [14. ☎️ Pruebas de llamadas desde CLI](#14-️-pruebas-de-llamadas-desde-cli)

---

# 1. 🧭 Navegación y ayuda CLI

```bash
help                              # Muestra todos los comandos disponibles
!                                 # Ejecuta comandos del sistema desde Asterisk CLI
cli show aliases                  # Muestra alias definidos en CLI
cli show permissions              # Muestra permisos de usuario actual
cli reload permissions            # Recarga configuración de permisos
core show sysinfo                 # Información del sistema
core show uptime                  # Tiempo de actividad del servicio Asterisk
core show version                 # Muestra versión actual de Asterisk
```

---

# 2. 📞 Extensiones PJSIP y SIP

### 🔹 PJSIP

```bash
pjsip show endpoints              # Lista todas las extensiones PJSIP
pjsip show contacts               # Muestra IP, puerto y estado de registro de extensiones
pjsip show endpoint 1001          # Información detallada de la extensión 1001
pjsip qualify 1001                # Envia un ping SIP (OPTIONS) a la extensión
pjsip show aors                   # Muestra Address of Record configurados
pjsip show auths                  # Lista autenticaciones configuradas
pjsip show transports             # Lista transportes SIP definidos (UDP, TCP, TLS)
```

### 🔹 SIP clásico (chan_sip)

```bash
sip show peers                    # Lista extensiones SIP configuradas
sip show peer 1001                # Detalles de conexión y autenticación del peer
sip show registry                 # Estado de registros SIP hacia ITSPs o PBX externas
sip qualify peer 1001             # Envía ping SIP clásico a la extensión
sip reload                        # Recarga configuración de chan_sip
```

### 🔹 IAX2

```bash
iax2 show peers                   # Lista extensiones o troncales IAX2
iax2 show registry                # Estado de registros IAX2
iax2 reload                       # Recarga configuración de IAX2
```

---

# 3. 📡 Troncales y registros externos

```bash
sip show registry                 # Registros SIP salientes
iax2 show registry                # Registros IAX salientes
dahdi show channels               # Canales físicos DAHDI
pri show spans                    # Estado de interfaces PRI (E1/T1)
```

---

# 4. 📊 Canales y llamadas

```bash
core show channels                # Llamadas activas con detalles
core show channels concise        # Llamadas activas en formato resumido
core show calls                   # Número de llamadas y canales activos
core show channel CHANNEL         # Información detallada de un canal específico
core show channelstats            # Estadísticas detalladas por canal
channel request hangup CHANNEL    # Finaliza una llamada específica
```

---

# 5. 🔁 Recargas, módulos y reinicios

```bash
core reload                       # Recarga completa de Asterisk sin reiniciar
dialplan reload                   # Recarga solo el dialplan (plan de marcado)
module show                       # Muestra módulos cargados
module reload res_pjsip.so        # Recarga solo el módulo PJSIP
core restart now                  # Reinicio inmediato de Asterisk
core stop gracefully              # Detiene Asterisk sin cortar llamadas activas
```

---

# 6. 📥 Colas y agentes

```bash
queue show                        # Estado de colas y agentes conectados
queue reload all                  # Recarga configuración de todas las colas
queue pause member PJSIP/1001     # Pausa al agente 1001
agent show all                    # Lista de agentes del sistema
agent logoff                      # Cierra sesión del agente actual
```

---

# 7. 🎙️ Conferencias

```bash
confbridge list                   # Muestra conferencias activas y participantes
confbridge mute all               # Silencia a todos los participantes
confbridge unmute all             # Reactiva el audio de todos los participantes
confbridge kick <ext>             # Expulsa una extensión de la sala
meetme list                       # Lista conferencias activas (modo legacy)
meetme kick <user>                # Expulsa un usuario específico de MeetMe
```

---

# 8. 🔊 Audio, RTP y Codecs

```bash
rtp set debug on                  # Muestra tráfico RTP en consola
rtp set debug off                 # Detiene visualización RTP
core show codecs audio            # Lista codecs de audio disponibles
core show sounds                  # Lista sonidos instalados en el sistema
file convert archivo.wav archivo.gsm  # Convierte archivos de audio
```

---

# 9. 🧠 ASTDB (Base de datos interna)

```bash
database show                     # Lista completa de la base de datos
database put family key value     # Guarda un valor en la base
database get family key           # Consulta un valor
database del family key           # Elimina un valor
```

---

# 10. 🔐 Seguridad, AMI y logs

```bash
manager show users                # Lista usuarios AMI definidos
manager show connected            # Conexiones AMI activas
pjsip show unidentified_requests  # Solicitudes SIP no identificadas
logger show channels              # Muestra canales de log activos
core set verbose 4                # Nivel de detalle en consola (0 a 10)
```

---

# 11. 🛠️ Extras y herramientas

```bash
fax show sessions                 # Sesiones de fax activas
fax show stats                    # Estadísticas generales de fax
http show status                  # Estado del servidor web embebido
xmldoc dump                       # Exporta documentación XML
dnsmgr status                     # Estado del gestor DNS
say load new                      # Activa el motor de locución nuevo
```

---

# 12. 🔑 Cambio de contraseñas

### 🔹 Extensión PJSIP

```ini
[1001]
type=auth
auth_type=userpass
password=NuevaContraseñaFuerte
```

### 🔹 Extensión SIP clásico

```ini
[1001]
secret=NuevaContraseñaSegura
```

### 🔹 Usuario AMI (manager.conf)

```ini
[admin]
secret=NuevaClaveAMI
read=all
write=all
```

```bash
asterisk -rx "module reload manager"
```

### 🔹 Usuario del sistema Linux

```bash
passwd root
passwd usuario
```

---

# 13. 🧩 Script de diagnóstico rápido

```bash
#!/bin/bash
echo "Canales activos:"
asterisk -rx "core show channels"
echo "Contactos registrados:"
asterisk -rx "pjsip show contacts"
echo "Colas activas:"
asterisk -rx "queue show"
echo "Estado del sistema:"
asterisk -rx "core show sysinfo"
```

---

# 14. ☎️ Pruebas de llamadas desde CLI

```bash
channel originate PJSIP/1001 extension@from-internal
```
> Llamada desde Asterisk hacia la extensión 1001 usando PJSIP.

```bash
channel originate SIP/1001 extension@from-internal
```
> Llamada usando SIP clásico (chan_sip).

```bash
channel originate Local/1002@from-internal extension@from-internal
```
> Simulación de llamada entre extensiones del mismo contexto.

```bash
channel originate PJSIP/troncal/999999999 extension@from-internal
```
> Llamada saliente por troncal (reemplazar nombre y número).

```bash
core show channels
```
> Verifica si la llamada está activa.

```bash
channel request hangup CHANNEL-ID
```
> Finaliza una llamada desde consola.
