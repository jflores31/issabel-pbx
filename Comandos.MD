# 🛠️ Manual Avanzado CLI - Issabel PBX / Asterisk (Navaja Suiza de Comandos)
**Autor:** Jesús Flores – Soporte TI  
**Versión:** 2025.04  
**Formato:** Markdown (.md)  
**Propósito:** Guía profesional de referencia para administración, monitoreo y solución de problemas en entornos Asterisk e Issabel desde CLI.

---

## 📑 ÍNDICE

1. Navegación y ayuda  
2. Extensiones PJSIP y SIP  
3. Troncales SIP/IAX/DAHDI  
4. Canales y llamadas  
5. Recargas, módulos y reinicios  
6. Colas y agentes  
7. Conferencias  
8. Audio, RTP y Codecs  
9. ASTDB (Base de datos interna)  
10. Seguridad, AMI y logs  
11. Extras y herramientas  
12. Cambio de contraseñas  
13. Script de diagnóstico rápido  
14. Pruebas de llamadas desde CLI

---

## 1. 🧭 Navegación y ayuda CLI

```bash
help                              # Muestra todos los comandos disponibles
!                                 # Ejecuta comandos del sistema desde Asterisk CLI
cli show aliases                  # Muestra alias definidos en CLI
cli show permissions              # Muestra permisos de usuario actual
cli reload permissions            # Recarga configuración de permisos
core show sysinfo                 # Información del sistema
core show uptime                  # Tiempo de actividad del servicio Asterisk
core show version                 # Muestra versión actual de Asterisk
