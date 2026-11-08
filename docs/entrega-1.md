# Entrega 1. Propuesta inicial del proyecto

**Proyecto:** Diseño e implantación de medidas de seguridad en la infraestructura informática de una asesoría  
**Alumno:** Jesús Conde  
**Centro:** IES Ribera del Tajo  
**Fecha de entrega:** 16 de octubre de 2026

> **Documento de propuesta.** Las situaciones, configuraciones y pruebas descritas son previstas; no representan trabajos ya ejecutados.

## 1. Título provisional

Diseño e implementación de una infraestructura informática segura para una asesoría mediante segmentación de red, control de accesos y protección de la información.

## 2. Empresa ficticia, actividad y situación inicial

Asesoría Tajo Gestión, S. L. es una empresa ficticia situada en Talavera de la Reina (Toledo), con una única oficina y 18 empleados distribuidos entre dirección/administración, asesoramiento fiscal-contable y gestión laboral. Atiende a autónomos y pequeñas empresas y maneja documentación tributaria, contable y laboral, contratos, nóminas y datos identificativos de clientes y empleados.

Se plantea que la asesoría comenzó con un equipo reducido y creció incorporando nuevos puestos y servicios sin revisar globalmente su red. En el escenario inicial, los equipos y servicios internos comparten una red poco segmentada; los permisos sobre documentación no están suficientemente diferenciados; no existe un procedimiento claro de acceso remoto y la visibilidad de los eventos de seguridad y la recuperación de archivos son limitadas. Estas condiciones son hipótesis del caso simulado, no hechos observados en una empresa real.

## 3. Problemas y necesidades de seguridad

La empresa necesita separar los accesos entre equipos de usuario y servicios internos, establecer reglas de comunicación justificadas y reducir la exposición innecesaria de los sistemas. También necesita controlar quién puede consultar o modificar la documentación laboral, fiscal y administrativa, así como permitir acceso remoto seguro a empleados autorizados.

La protección de datos requiere contemplar confidencialidad, integridad y disponibilidad: autenticación y permisos por perfil, comunicaciones protegidas, registros útiles para revisar incidencias, copias de seguridad y pruebas de restauración. Se utilizarán únicamente datos ficticios. El proyecto considerará principios del RGPD y la LOPDGDD, sin pretender certificar el cumplimiento legal completo de la organización.

## 4. Objetivos

**Objetivo general:** diseñar y validar, en un laboratorio virtualizado, una solución de seguridad proporcionada a las necesidades de una asesoría pequeña, integrando administración de sistemas, redes, servicios y protección de información.

**Objetivos específicos:** (a) analizar el escenario inicial y establecer requisitos; (b) diseñar redes diferenciadas y políticas de filtrado; (c) desplegar un servicio documental web interno y gestionar usuarios y permisos; (d) habilitar, si los recursos lo permiten, acceso remoto mediante VPN; (e) definir y probar copias de seguridad y restauración; (f) registrar y revisar eventos relevantes; (g) documentar configuraciones, incidencias, decisiones y resultados mediante GitHub.

## 5. Soluciones previstas

Se propone un laboratorio con un firewall virtual que conecte y filtre dos redes lógicas: usuarios y servidores. En la red de servidores se desplegará Linux con una aplicación web documental existente, sin desarrollar software propio. Se estudiará File Browser u otra alternativa ligera, con cuentas de prueba y permisos diferenciados por función. La intranet será de uso interno; el acceso remoto, cuando se implante, se realizará por VPN y no mediante exposición pública directa del servicio.

Se aplicarán medidas básicas de bastionado (servicios mínimos, actualizaciones, credenciales y permisos adecuados), reglas de acceso por necesidad, registro de conexiones y un procedimiento de respaldo y recuperación con datos de ejemplo. Se prepararán pruebas positivas y negativas: acceso permitido a la intranet, denegación de conexiones no autorizadas, diferencias de permisos entre perfiles y recuperación de archivos.

## 6. Tecnologías y herramientas previstas

VMware en Windows 11 para la virtualización; OPNsense como firewall y, si resulta viable, VPN y registros; una distribución Linux de servidor (por ejemplo, Debian) para alojar el servicio documental; File Browser o herramienta equivalente para el acceso web a documentos; herramientas de sistema para copias de seguridad y restauración; GitHub y Markdown para documentación y seguimiento. La selección definitiva se confirmará tras pruebas de compatibilidad y consumo.

El equipo disponible cuenta con Intel Core i7-1065G7, 12 GB de RAM y aproximadamente 39 GB de disco libre. Por esta limitación se prevé ejecutar pocas máquinas simultáneamente, emplear instalaciones mínimas y aprovechar el anfitrión como cliente de pruebas cuando sea posible. No se presupone disponer de almacenamiento adicional.

## 7. Alcance y exclusiones

Alcance comprometido en la propuesta: análisis y diseño del escenario, laboratorio mínimo con firewall y servidor Linux, dos zonas de red diferenciadas, aplicación documental accesible desde un cliente de prueba, usuarios/permisos, reglas de filtrado, bastionado básico, documentación y pruebas verificables. Se prevén además VPN, registros y respaldo/restauración, cuya implementación concreta se ajustará tras medir recursos y validar la arquitectura con la profesora.

Quedan fuera el despliegue en una empresa real, el uso de datos personales auténticos, el desarrollo de una aplicación propia, la alta disponibilidad, un portal público de clientes, la implantación completa de Active Directory, un SIEM empresarial y la certificación de cumplimiento normativo. Un IDS/IPS podrá considerarse como mejora futura, pero no será requisito de la versión final. Si el espacio libre impide ejecutar el laboratorio mínimo, se revisará el alcance con la profesora antes de comprometer nuevos componentes.

## 8. Planificación temporal hasta diciembre

| Periodo orientativo | Actividades y evidencias |
|---|---|
| 14–16 octubre | Definición del caso, necesidades, objetivos y propuesta inicial. |
| 19–25 octubre | Requisitos, diseño de redes, arquitectura, análisis de riesgos y planificación. |
| 26 octubre–8 noviembre | Repositorio, preparación y prueba de viabilidad del entorno virtual. |
| 9–22 noviembre | Firewall, servidor documental, cuentas, permisos y filtrado. |
| 23 noviembre–6 diciembre | VPN y copias según viabilidad; logs, bastionado y pruebas documentadas. |
| Diciembre, antes de la defensa | Corrección de incidencias, memoria final, evidencias y preparación de la presentación. |

El calendario es provisional y se adaptará a las entregas que como tutora me indiques. El seguimiento se realizará con hitos, historial de cambios en GitHub, registro de incidencias y evidencias de las pruebas. La defensa se apoyará en diagramas, capturas y una demostración breve si el entorno funciona con estabilidad.
