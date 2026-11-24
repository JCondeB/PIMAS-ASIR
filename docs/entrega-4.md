# Entrega 4 — Implementación de servicios y seguridad

## 1. Introducción

En esta fase del proyecto PIMAS de Asesoría Tajo Gestión, S. L., se han implementado los servicios internos sobre la infraestructura virtual desplegada en la entrega anterior.

## 2. Infraestructura de red

| Elemento | Configuración |
|---|---|
| Cortafuegos | OPNsense |
| Servidor | Ubuntu Server |
| Cliente | Windows 11 |
| Red de usuarios | 192.168.10.0/24 |
| Red de servidores | 192.168.20.0/24 |
| Dirección del servidor | 192.168.20.10 |
| Virtualización | VMware Workstation |

## 3. Servicios implementados

- Apache como servidor web.
- MariaDB como sistema gestor de bases de datos.
- GLPI como aplicación de gestión de incidencias.
- Samba como servicio documental compartido.
- SSH para la administración remota.

## 4. Instalación y configuración de GLPI

Se ha instalado GLPI sobre Ubuntu Server, utilizando Apache y MariaDB. Se ha comprobado su funcionamiento desde el equipo cliente Windows 11.

![E4-10 - Acceso a GLPI](imagenes/entrega4/E4-10.png)

## 5. Administración de usuarios y perfiles

Se han configurado usuarios con diferentes niveles de acceso en GLPI, comprobando el funcionamiento del perfil Self-Service mediante un usuario de prueba.

![E4-11 - Gestion de usuarios](imagenes/entrega4/E4-11.png)

![E4-12 - Acceso con perfil Self-Service](imagenes/entrega4/E4-12.png)

## 6. Servicio documental y permisos

Se ha instalado Samba y se han creado dos carpetas compartidas con permisos diferenciados.

| Carpeta | Grupo autorizado | Permisos |
|---|---|---|
| General | empleados | 2770 |
| Confidencial | direccion | 2770 |

Se han creado los usuarios empleado1 y direccion1 para gestionar los accesos según su pertenencia a los grupos correspondientes.

![E4-13 - Estructura y permisos](imagenes/entrega4/E4-13.png)

![E4-14 - Acceso autorizado](imagenes/entrega4/E4-14.png)

![E4-15 - Acceso denegado](imagenes/entrega4/E4-15.png)

## 7. Reglas de cortafuegos

Se han configurado reglas en OPNsense para restringir el tráfico IPv4 entre la red de usuarios y la red de servidores.

| Servicio | Protocolo | Puerto | Accion |
|---|---|---|---|
| GLPI | TCP | 80 | Permitir |
| Samba | TCP | 445 | Permitir |
| SSH desde equipo autorizado | TCP | 22 | Permitir |
| Resto del trafico IPv4 hacia servidores | Cualquiera | Todos | Bloquear |

![E4-16 - Reglas de OPNsense](imagenes/entrega4/E4-16.png)

### Prueba de conexion permitida

Se comprueba el acceso al puerto TCP/80 mediante PowerShell y los registros de OPNsense.

![E4-17a - Conexion permitida](imagenes/entrega4/E4-17a.png)

![E4-17b - Registro de conexion permitida](imagenes/entrega4/E4-17b.png)

### Prueba de conexion bloqueada

Se comprueba el bloqueo del acceso directo al puerto TCP/3306 de MariaDB desde la red de usuarios.

![E4-18a - Conexion bloqueada](imagenes/entrega4/E4-18a.png)

![E4-18b - Registro de conexion bloqueada](imagenes/entrega4/E4-18b.png)

## 8. Pruebas funcionales

| Prueba | Resultado |
|---|---|
| Acceso a GLPI | Correcto |
| Conexion GLPI-MariaDB | Correcto, GLPI operativo |
| Acceso segun perfil GLPI | Correcto |
| Acceso autorizado a carpeta | Correcto |
| Acceso denegado a carpeta | Correcto |
| Filtrado de OPNsense | Correcto en las pruebas realizadas |

## 9. Incidencias y soluciones

- Error 404 en GLPI: revision de la configuracion de Apache.
- Error 500 en GLPI: ajuste de la configuracion de reescritura de Apache.
- Error de registro de usuario en Samba: verificacion del usuario en Ubuntu y repeticion del registro.

Las incidencias se han solucionado y los servicios se encuentran operativos.

## 10. Estado del proyecto y siguientes pasos

Se ha completado la implementacion de los principales servicios internos, la gestion de usuarios, los permisos documentales y el filtrado de red mediante OPNsense.

GLPI funciona actualmente mediante HTTP. Queda pendiente valorar la configuracion de HTTPS, implementar las copias de seguridad y comprobar su restauracion, asi como completar las medidas de seguridad y las pruebas finales del proyecto.
