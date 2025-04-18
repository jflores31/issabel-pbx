# 🛠️ Manual Avanzado CLI - Issabel PBX / Asterisk (Navaja Suiza de Comandos)
**Autor:** Jesús Flores – Soporte TI  
**Versión:** 2025.04  
**Formato:** Markdown (.md)  
**Propósito:** Herramienta de referencia completa para administrar, monitorear, auditar y resolver incidencias técnicas en sistemas Issabel PBX / Asterisk CLI.

---

## 📑 ÍNDICE

1. [🧭 Navegación y ayuda](#1)
2. [📞 Extensiones PJSIP y SIP](#2)
3. [📡 Troncales SIP/IAX/DAHDI](#3)
4. [📊 Canales y llamadas](#4)
5. [🔁 Recargas, módulos y reinicios](#5)
6. [📥 Colas y agentes](#6)
7. [🎙️ Conferencias](#7)
8. [🔊 Audio, RTP, Codecs](#8)
9. [🧠 ASTDB (Base de datos interna)](#9)
10. [🔐 Seguridad, AMI y logs](#10)
11. [🛠️ Extras y otros servicios](#11)
12. [🧩 Script de diagnóstico](#12)

---

## 1. 🧭 Navegación y ayuda CLI

```bash
help                              # Muestra todos los comandos
!                                 # Ejecutar comando del sistema
cli show aliases                  # Ver alias definidos
cli show permissions              # Ver permisos de usuario CLI
cli reload permissions            # Recargar permisos
core show sysinfo                 # Ver detalles del sistema
core show uptime                  # Tiempo activo
core show version                 # Versión de Asterisk
2. 📞 Extensiones PJSIP y SIP
🔹 PJSIP
bash
Copiar
Editar
pjsip show endpoints              # Lista todas las extensiones
pjsip show contacts               # Ver contactos activos
pjsip show endpoint 1001          # Ver detalles de la ext. 1001
pjsip qualify 1001                # Hacer ping OPTIONS
pjsip show aors                   # Ver AORs configurados
pjsip show auths                  # Ver autenticaciones
pjsip show transports             # Transportes disponibles
🔹 SIP clásico (chan_sip)
bash
Copiar
Editar
sip show peers                    # Lista extensiones SIP
sip show peer 1001                # Detalles de ext. SIP
sip show registry                 # Registros SIP salientes
sip qualify peer 1001             # Ping a peer SIP
sip reload                        # Recargar chan_sip
🔹 IAX2
bash
Copiar
Editar
iax2 show peers                   # Lista peers IAX2
iax2 show registry                # Registro IAX2
iax2 reload                       # Recarga IAX
3. 📡 Troncales y registros externos
bash
Copiar
Editar
sip show registry                 # Estado de registros SIP
iax2 show registry                # Estado de IAX
dahdi show channels               # Canales DAHDI
pri show spans                    # Interfaces PRI
4. 📊 Canales y llamadas
bash
Copiar
Editar
core show channels                # Canales activos
core show channels concise        # Vista resumida
core show calls                   # Llamadas en curso
core show channel CHANNEL         # Detalles de un canal
core show channelstats            # Estadísticas
channel request hangup CHANNEL    # Finalizar canal
5. 🔁 Recargas, módulos y reinicios
bash
Copiar
Editar
core reload                       # Recarga completa
dialplan reload                   # Solo recarga plan de marcado
module show                       # Muestra módulos cargados
module reload res_pjsip.so        # Recarga un módulo específico
core restart now                  # Reinicio inmediato
core stop gracefully              # Detener Asterisk de forma segura
6. 📥 Colas y agentes
bash
Copiar
Editar
queue show                        # Ver colas y miembros
queue reload all                  # Recargar colas
queue pause member PJSIP/1001     # Pausar agente
agent show all                    # Ver agentes
agent logoff                      # Cerrar sesión de agente
7. 🎙️ Conferencias
bash
Copiar
Editar
confbridge list                   # Ver conferencias activas
confbridge mute/unmute all        # Control de audio
confbridge kick <ext>             # Expulsar participante
meetme list                       # Ver conferencias MeetMe
meetme kick <user>                # Expulsar usuario
8. 🔊 Audio, RTP y codecs
bash
Copiar
Editar
rtp set debug on                  # Activar debug RTP
rtp set debug off                 # Desactivar debug RTP
core show codecs audio            # Ver codecs de audio
core show sounds                  # Archivos de sonido
file convert foo.wav bar.gsm      # Convertir audio
9. 🧠 ASTDB (Base de datos interna)
bash
Copiar
Editar
database show                     # Mostrar base completa
database put family key value     # Escribir
database get family key           # Leer valor
database del family key           # Eliminar valor
10. 🔐 Seguridad, AMI y debugging
bash
Copiar
Editar
manager show users                # Ver usuarios AMI
manager show connected            # Usuarios conectados al AMI
pjsip show unidentified_requests  # SIP desconocidos (posible intento ataque)
logger show channels              # Ver canales de log
core set verbose 4                # Nivel de salida de consola
11. 🛠️ Extras y herramientas
bash
Copiar
Editar
fax show sessions                 # Sesiones de fax activas
fax show stats                    # Estadísticas de fax
http show status                  # Estado de servidor web interno
xmldoc dump                       # Volcar documentación XML
dnsmgr status                     # Ver estado DNS
say load new                      # Establecer modo de locución
12. 🧩 Script de diagnóstico rápido (diagnostico.sh)
bash
Copiar
Editar
#!/bin/bash
echo "Canales activos:"
asterisk -rx "core show channels"
echo "Contactos registrados:"
asterisk -rx "pjsip show contacts"
echo "Colas:"
asterisk -rx "queue show"
echo "Sistema:"
asterisk -rx "core show sysinfo"