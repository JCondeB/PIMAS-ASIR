# Entrega 3. Preparación del entorno de virtualización y repositorio

**Proyecto:** Asesoría Tajo Gestión, S. L.  
**Alumno:** Jesús Conde  
**Fecha:** 8 de noviembre de 2026

> Documentación del despliegue y las pruebas del laboratorio. La configuración de GitHub y la publicación web se han completado posteriormente a la redacción inicial del informe.

Contexto y objetivos

Esta tercera fase continúa el diseño aprobado para la asesoría ficticia de 18 empleados. Se ha desplegado un laboratorio mediante VMware Workstation 16 Pro, con OPNsense como cortafuegos y Ubuntu Server como servidor de la infraestructura. El entorno dispone de redes diferenciadas para usuarios y servidores, con comunicación a través de OPNsense.

Los objetivos de esta fase son instalar y configurar ambas máquinas virtuales, establecer el direccionamiento y las rutas de red, verificar la conectividad interna y externa, comprobar la administración remota mediante SSH y documentar las incidencias y soluciones aplicadas. Se han completado las pruebas de funcionamiento simultáneo, consumo de recursos y filtrado de tráfico. Queda pendiente la publicación del repositorio GitHub. La instalación de GLPI y MariaDB se realizará en fases posteriores.

## 2. Equipo anfitrión y viabilidad inicial

| Recurso | Comprobación inicial |
| --- | --- |
| Equipo | Portátil Windows 11; Intel Core i7-1065G7, 4 núcleos y 8 hilos |
| Memoria | 12 GB instalados; 5,3 GB utilizados (45 %) tras reinicio |
| Disco | SSD NVMe de 475 GB; 271 GB libres tras retirar máquinas antiguas |
| Virtualización | Habilitada en el Administrador de tareas |
| Hipervisor | VMware Workstation 16 Pro, versión 16.2.4, build 20089737 |

El equipo anfitrión dispone de los recursos necesarios para el despliegue inicial del laboratorio. Se ha completado la instalación de OPNsense y Ubuntu Server en VMware Workstation 16 Pro, comprobándose el funcionamiento de ambos sistemas y la comunicación entre sus respectivas redes.

La viabilidad del entorno se ha verificado mediante pruebas de conectividad interna, acceso a Internet y administración remota por SSH. También se ha comprobado el funcionamiento simultáneo de ambas máquinas virtuales y el consumo de recursos del equipo anfitrión. Durante las pruebas se registró una utilización de memoria RAM del 96 % (11,2 GB), mientras que el consumo de CPU y la actividad del disco fueron reducidos. Este elevado uso de memoria constituye una limitación que deberá tenerse en cuenta al incorporar nuevos servicios en fases posteriores.

![Evidencia 01 de la Entrega 3](imagenes/evidencia-01.png)

Características del equipo anfitrión utilizado para el desarrollo del laboratorio, incluyendo procesador, memoria RAM y arquitectura del sistema.

![Evidencia 02 de la Entrega 3](imagenes/evidencia-02.png)

Espacio de almacenamiento disponible en el equipo anfitrión tras la eliminación de las máquinas virtuales anteriores.

![Evidencia 03 de la Entrega 3](imagenes/evidencia-03.png)

Comprobación de la virtualización por hardware habilitada y del consumo de memoria RAM del equipo anfitrión antes del despliegue del laboratorio.

![Evidencia 04 de la Entrega 3](imagenes/evidencia-04.png)

Identificación de VMware Workstation 16 Pro, versión 16.2.4, utilizado como plataforma de virtualización del laboratorio.

## 3. Diseño de redes virtuales

![Evidencia 05 de la Entrega 3](imagenes/evidencia-05.png)

Configuración inicial de las redes virtuales disponibles en VMware Workstation antes de crear las redes del laboratorio.

La infraestructura de red del laboratorio se ha configurado mediante tres segmentos virtuales independientes en VMware Workstation: una red WAN para el acceso a Internet mediante NAT, una red de usuarios (192.168.10.0/24) y una red de servidores (192.168.20.0/24).

OPNsense actúa como cortafuegos y dispositivo de enrutamiento entre ambas redes internas, permitiendo controlar las comunicaciones mediante reglas de filtrado.

| Red | Configuración aplicada | Función |
| --- | --- | --- |
| WAN | VMnet8 / NAT | Salida de laboratorio; IP asignada por DHCP |
| Usuarios | VMnet2 / host-only | Windows anfitrión como cliente; 192.168.10.0/24 |
| Servidores | VMnet3 / host-only | Ubuntu Server; 192.168.20.0/24 |

El direccionamiento configurado es el siguiente: OPNsense dispone de la dirección 192.168.10.1/24 en LAN (usuarios), 192.168.20.1/24 en OPT1 (servidores) y una dirección obtenida mediante DHCP en WAN (VMnet8). Ubuntu Server utiliza la dirección estática 192.168.20.10/24, con puerta de enlace 192.168.20.1.

Se ha deshabilitado el servicio DHCP de VMware en las redes internas. La red de servidores VMnet3 no dispone de conexión directa con el anfitrión Windows 11. Para permitir la comunicación desde Windows hacia Ubuntu Server se ha configurado una ruta estática hacia 192.168.20.0/24 a través de OPNsense (192.168.10.1), verificándose posteriormente la conectividad mediante ICMP y SSH.

![Evidencia 06 de la Entrega 3](imagenes/evidencia-06.png)

Configuración de la red virtual VMnet2 para los usuarios de la asesoría, con direccionamiento 192.168.10.0/24, adaptador del anfitrión habilitado y servicio DHCP de VMware desactivado

![Evidencia 07 de la Entrega 3](imagenes/evidencia-07.png)

Configuración de la red virtual VMnet3 destinada a servidores, con direccionamiento 192.168.20.0/24, sin DHCP de VMware ni conexión directa al anfitrión.

![Evidencia 08 de la Entrega 3](imagenes/evidencia-08.png)

Configuración de la red NAT VMnet8, utilizada como conexión WAN del firewall OPNsense.

## 4. Creación de las máquinas virtuales

| Máquina | Configuración final | Conexiones |
| --- | --- | --- |
| OPNsense | 3 GB RAM; 2 vCPU; disco virtual 20 GB | WAN VMnet8; usuarios VMnet2; servidores VMnet3 |
| Ubuntu Server | 3 GB RAM; 2 vCPU; disco virtual 35 GB | Únicamente VMnet3;  |

Se han creado e instalado las dos máquinas virtuales previstas utilizando VMware Workstation 16 Pro. Durante la instalación de OPNsense fue necesario ampliar la memoria RAM inicialmente asignada de 2 a 3 GB para cumplir los requisitos del instalador. Ubuntu Server se configuró con 3 GB de RAM, dos procesadores virtuales y un disco de 35 GB.

Ambos sistemas han quedado instalados y operativos, con sus interfaces de red configuradas y conectividad verificada. Ubuntu Server dispone además del servicio OpenSSH, que permite su administración remota desde Windows 11.

![Evidencia 09 de la Entrega 3](imagenes/evidencia-09.png)

![Evidencia 10 de la Entrega 3](imagenes/evidencia-10.png)

Configuración inicial de hardware de la máquina virtual PIMAS-OPNsense en VMware Workstation, con 2 GB de memoria RAM, dos núcleos de procesamiento y tres adaptadores de red destinados a WAN, usuarios y servidores. Posteriormente, la memoria RAM se amplió a 3 GB para completar la instalación.

![Evidencia 11 de la Entrega 3](imagenes/evidencia-11.png)

Máquina virtual PIMAS-OPNsense creada en VMware Workstation 16 Pro, inicialmente con 2 GB de memoria RAM, dos núcleos virtuales, disco SCSI de 20 GB y tres adaptadores de red conectados a VMnet8 (WAN), VMnet2 (usuarios) y VMnet3 (servidores). La configuración final utiliza 3 GB de RAM.

La máquina virtual PIMAS-OPNsense se creó con tres adaptadores de red conectados a VMnet8 (WAN), VMnet2 (LAN) y VMnet3 (OPT1). Tras completar la instalación de OPNsense 26.7, se configuraron las interfaces con direccionamiento DHCP en WAN y direcciones estáticas en las redes internas: 192.168.10.1/24 para usuarios y 192.168.20.1/24 para servidores.

Se verificó el acceso a la interfaz web de administración desde Windows 11 mediante HTTPS y se configuraron las reglas necesarias para permitir las comunicaciones comprobadas entre los segmentos de red.

Durante la instalación de OPNsense 26.7 se detectó que el instalador UFS recomendaba disponer de al menos 3 GB de memoria RAM. La configuración inicial de la máquina virtual contemplaba 2 GB, por lo que se decidió aumentar la asignación a 3 GB antes de continuar con la instalación. La ampliación permitió continuar y completar correctamente la instalación del cortafuegos.

![Evidencia 12 de la Entrega 3](imagenes/evidencia-12.png)

Consola de OPNsense 26.7 tras la instalación, con las interfaces WAN, LAN y OPT1 asignadas y la dirección IPv4 de LAN configurada.

![Evidencia 13 de la Entrega 3](imagenes/evidencia-13.png)

Configuración final de las interfaces de OPNsense 26.7, con WAN mediante DHCP y direccionamiento estático en las redes de usuarios (192.168.10.0/24) y servidores (192.168.20.0/24).

![Evidencia 14 de la Entrega 3](imagenes/evidencia-14.png)

Configuración inicial de la máquina virtual PIMAS-UbuntuServer en VMware Workstation, con 3 GB de memoria RAM, dos núcleos de procesamiento, disco virtual de 35 GB y conexión a la red de servidores VMnet3.

![Evidencia 15 de la Entrega 3](imagenes/evidencia-15.png)

Ubuntu Server 26.04.1 LTS iniciado correctamente en VMware Workstation, con la interfaz de red ens33 activa y la dirección IPv4 estática 192.168.20.10/24 configurada.

![Evidencia 16 de la Entrega 3](imagenes/evidencia-16.png)

Verificación de la conectividad entre Ubuntu Server y el firewall OPNsense mediante ICMP, con cuatro paquetes recibidos y un 0 % de pérdida.

![Evidencia 17 de la Entrega 3](imagenes/evidencia-17.png)

Verificación de la conectividad externa de Ubuntu Server a través del firewall OPNsense, mediante una prueba ICMP a 1.1.1.1 con un 0 % de pérdida de paquetes.

![Evidencia 18 de la Entrega 3](imagenes/evidencia-18.png)

Comprobación de la resolución DNS y conectividad externa de Ubuntu Server a través de OPNsense, mediante consultas al dominio archive.ubuntu.com, con un 100 % de paquetes recibidos.

![Evidencia 19 de la Entrega 3](imagenes/evidencia-19.png)

Verificación del acceso de Ubuntu Server a los repositorios oficiales mediante APT, a través del firewall OPNsense, con descarga correcta de los índices de paquetes.

![Evidencia 20 de la Entrega 3](imagenes/evidencia-20.png)

Verificación de la conectividad de Ubuntu Server tras la actualización del sistema. Se comprueba la comunicación con OPNsense (192.168.20.1), el acceso a Internet (1.1.1.1) y la resolución DNS mediante archive.ubuntu.com, con un 0 % de pérdida de paquetes.

![Evidencia 21 de la Entrega 3](imagenes/evidencia-21.png)

Verificación de la conectividad entre Windows 11 y Ubuntu Server a través de OPNsense. Tras configurar una ruta estática hacia la red 192.168.20.0/24, se comprueba la comunicación mediante ICMP con cuatro respuestas satisfactorias y un 0 % de pérdida de paquetes.

![Evidencia 22 de la Entrega 3](imagenes/evidencia-22.png)

Acceso remoto mediante SSH desde Windows 11 a Ubuntu Server (192.168.20.10), utilizando el usuario pimasadmin. Se verifica la autenticación y el establecimiento de una sesión de administración remota a través de OPNsense.

## 5. Configuración inicial y verificación

Una vez instalados OPNsense 26.7 y Ubuntu Server 26.04.1 LTS, se realizaron pruebas de configuración y conectividad para verificar el funcionamiento de la infraestructura virtual.

Se comprobó el direccionamiento de las interfaces, la comunicación entre Ubuntu Server y OPNsense, el acceso a Internet desde la red de servidores y la conectividad desde Windows 11 hacia ambas redes internas.

Las pruebas se realizaron utilizando OPNsense como dispositivo de enrutamiento entre las redes de usuarios y servidores. También se verificó el acceso remoto a Ubuntu Server mediante SSH desde el equipo anfitrión.

| ID | Prueba prevista | Evidencia y resultado |
| --- | --- | --- |
| P01 | Arranque simultáneo de OPNsense y Ubuntu | Correcto. Ambas máquinas virtuales operativas simultáneamente. |
| P02 | IP de interfaces y rutas | Correcto. LAN: 192.168.10.1; OPT1: 192.168.20.1; Ubuntu: 192.168.20.10. |
| P03 | Windows alcanza interfaz de usuarios de OPNsense | Correcto. Ping y acceso HTTPS a 192.168.10.1. |
| P04 | Ubuntu alcanza gateway de servidores | Correcto: 0 % de pérdida |
| P05 | Acceso de usuarios a servidor a través de OPNsense | Correcto. Ping y conexión SSH desde Windows 11. |
| P06 | Bloqueo de tráfico no autorizado | Correcto. Conexión TCP al puerto 22 bloqueada y registrada en OPNsense. |
| P07 | Consumo de RAM y disco con ambas VMs activas | Verificado. RAM al 96 % (11,2 GB utilizados). CPU y actividad de disco reducidas. |

![Evidencia 23 de la Entrega 3](imagenes/evidencia-23.png)

Verificación del bloqueo de tráfico no autorizado mediante OPNsense. El registro del cortafuegos muestra el rechazo de conexiones TCP desde Ubuntu Server (192.168.20.10) hacia el puerto 22 del destino 1.1.1.1, conforme a la política de denegación predeterminada.

![Evidencia 24 de la Entrega 3](imagenes/evidencia-24.png)

![Evidencia 25 de la Entrega 3](imagenes/evidencia-25.png)

Las pruebas realizadas han confirmado la comunicación entre las redes de usuarios y servidores a través de OPNsense. Se ha verificado la conectividad de Ubuntu Server con su puerta de enlace, el acceso a Internet, la resolución de nombres DNS y el establecimiento de una conexión SSH desde Windows 11 hacia Ubuntu Server.

Asimismo, se ha comprobado el funcionamiento de las reglas del cortafuegos mediante una prueba de conexión TCP al puerto 22, cuyo bloqueo quedó registrado en OPNsense conforme a la política de denegación predeterminada.

Finalmente, se evaluó el consumo de recursos del equipo anfitrión durante la ejecución simultánea de las máquinas virtuales. Se registró una utilización de memoria RAM del 96 %, mientras que el consumo de CPU y la actividad del disco fueron reducidos. Esta limitación de memoria deberá tenerse en cuenta en las siguientes fases del proyecto, especialmente durante la incorporación de nuevos servicios.

## 6. Incidencias y cambios

| Fecha | Incidencia / cambio | Solución / estado |
| --- | --- | --- |
| Antes del despliegue | Almacenamiento insuficiente para laboratorio | Se retiraron máquinas virtuales antiguas; 271 GB libres. |
| Durante la instalación | Memoria asignada inicialmente insuficiente para OPNsense | Se amplió la RAM asignada de 2 a 3 GB. |
| Durante la configuración de red | Ubuntu no alcanzaba inicialmente la interfaz OPT1 | Se revisaron las reglas ICMP; comunicación con 192.168.20.1 verificada. |
| Durante las pruebas de conectividad | Ubuntu no accedía inicialmente a Internet | Se revisaron reglas y ruta predeterminada; ping a 1.1.1.1 correcto. |
| Durante las pruebas DNS | Fallo temporal de resolución de nombres y acceso a repositorios | Se revisaron las reglas y la conectividad; DNS y actualización de Ubuntu verificados. |
| Durante las pruebas desde Windows | Windows no alcanzaba Ubuntu Server | Se añadió ruta a 192.168.20.0/24 vía 192.168.10.1; ping y SSH correctos. |
| Durante la validación del firewall | Verificación del bloqueo de tráfico no autorizado | Conexión TCP al puerto 22 de 1.1.1.1 bloqueada y registrada en OPNsense. |
| Durante la medición de recursos | Consumo elevado de RAM del anfitrión | Se registraron 11,2 GB en uso (96 %); limitación pendiente de optimizar. |

Durante la preparación del laboratorio se produjeron incidencias relacionadas con la asignación de recursos, la configuración de las interfaces de red, el enrutamiento y las reglas del cortafuegos.

Las incidencias de conectividad se resolvieron mediante la revisión de las configuraciones de VMware, OPNsense y Ubuntu Server, verificándose posteriormente la comunicación entre redes, el acceso a Internet, la resolución DNS y la administración remota mediante SSH.

La principal limitación pendiente es el elevado consumo de memoria RAM del equipo anfitrión, que alcanzó el 96 % durante la ejecución simultánea de ambas máquinas virtuales. Será necesario tener en cuenta esta circunstancia al incorporar nuevos servicios en fases posteriores.

## 7. Repositorio GitHub

Se ha creado el repositorio propio del proyecto y configurado la publicación automática de la documentación con Markdown, MkDocs, Material for MkDocs, GitHub Actions y GitHub Pages.

- **Repositorio:** https://github.com/JCondeB/PIMAS-ASIR
- **Documentación web:** https://jcondeb.github.io/PIMAS-ASIR/

No se publican discos de máquinas virtuales, contraseñas ni claves privadas.

## 8. Resultados y siguientes pasos

Se ha completado el despliegue inicial del laboratorio de virtualización mediante VMware Workstation 16 Pro, con las máquinas virtuales OPNsense 26.7 y Ubuntu Server 26.04.1 LTS.

Se han configurado tres redes diferenciadas: WAN (VMnet8), usuarios (VMnet2) y servidores (VMnet3). Las pruebas realizadas confirman el funcionamiento simultáneo de ambas máquinas virtuales, la conectividad entre redes, el acceso a Internet desde Ubuntu Server y la administración remota mediante SSH desde Windows 11.

También se ha verificado el funcionamiento de las reglas del cortafuegos mediante pruebas de tráfico permitido y bloqueado, así como el registro de las conexiones denegadas.

La principal limitación detectada es el consumo de memoria RAM del equipo anfitrión, que alcanzó el 96 % (11,2 GB) durante el funcionamiento simultáneo de las máquinas virtuales.

En la siguiente fase se abordará la instalación de GLPI y MariaDB, la configuración de usuarios y permisos y la incorporación de los servicios y medidas de seguridad previstos en el diseño del proyecto.
