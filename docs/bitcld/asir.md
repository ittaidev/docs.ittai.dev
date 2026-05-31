# Relación de los módulos cursados con el proyecto

BitCLD fue pensado para que pudiéramos aplicar de la forma más práctica posible todos los conocimientos adquiridos en el ciclo tocando y profundizando en el mayor número de módulos posibles. Además también buscábamos que fuera un proyecto que pudiera tener una utilidad real una vez finalizado el curso.

A continuación mostramos cada tecnología con su relación correspondiente a los principales módulos que se ha trabajado en cada una bajo nuestra opinión:


##	Servidor Ubuntu

- Implantación de Sistemas Operativos (ISO) → Instalación, configuración y administración del sistema operativo de nuestro equipo.
- Planificación y Administración de Redes (PAR) → Asignación de IP fija, gestión de interfaces y rutas.
- Administración de Sistemas Operativos (ASO) → Gestión de usuarios, servicios, permisos y monitorización del sistema.
- Fundamentos de Hardware (FH) → Configuración y mantenimiento del minipc físico donde se aloja el servidor.


## Docker y dockge

- Implantación de Aplicaciones Web (IAW) → Despliegue de aplicaciones mediante contenedores.
- Administración de Sistemas Operativos (ASO) → Automatización del despliegue y administración de los entornos virtualizados.
- Implantación de Sistemas Operativos (ISO) → Configuración del entorno base y servicios del servidor.
- Planificación y Administración de Redes (PAR) → Creación de redes internas para comunicación entre los distintos contenedores.


## Cloudflare, Cloudflared tunnel, Access y Pages

- Servicios de Red e Internet (SRI) → Publicación de servicios internos sin exponer puertos, resolución de nombres y configuración de dominios
- Seguridad y Alta Disponibilidad (SAD) → Túneles cifrados y autenticación a través de Cloudflare, autenticación de usuarios mediante Zero Trust Access
- Planificación y Administración de Redes (PAR) → Enrutamiento seguro entre red local y externa.
- Ciberseguridad (CIBER) →  Protección frente a ataques DDoS y spoofing DNS
- Planificación y Administración de Redes (PAR) → Administración avanzada de zonas DNS y registros.


##	Caddy server
- Servicios de Red e Internet (SRI) → Servidor web y proxy inverso.
- Planificación y Administración de Redes (PAR) → Gestión de tráfico interno (.lan) y certificados.
- Seguridad y Alta Disponibilidad (SAD) → Encriptación TLS, aislamiento de servicios y gestión de acceso local.


##	Tailscale
- Planificación y Administración de Redes (PAR) → Creación de una red privada virtual entre dispositivos.
- Seguridad y Alta Disponibilidad (SAD) → Cifrado de extremo a extremo y control de acceso seguro.
- Administración de Sistemas Operativos (ASO) → Integración con el sistema anfitrión y conexión remota por SSH.


##	Uptime Kuma
- Seguridad y Alta Disponibilidad (SAD) → Monitorización del estado de los servicios, disponibilidad y notificaciones.
- Administración de Sistemas Operativos (ASO) → Análisis de logs y respuesta ante caídas o errores.

##	Nextcloud

- Servicios de Red e Internet (SRI) → Implementación de un servicio web y sincronización de archivos.
- Administración de Sistemas Operativos (ASO) → Mantenimiento, actualizaciones y copias de seguridad del servicio.
- Seguridad y Alta Disponibilidad (SAD) → Control de acceso, cifrado, gestión de usuarios y permisos.


##	Vaultwarden
- Seguridad y Alta Disponibilidad (SAD) → Cifrado extremo a extremo, control de accesos y autenticación.
- Administración de Sistemas Operativos (ASO) → Gestión de entornos seguros en contenedor.


##	Adguard Home
- Planificación y Administración de Redes (PAR) → Configuración del servidor DNS y resolución de nombres.
- Seguridad y Alta Disponibilidad (SAD) y ciberseguridad (CIBER) → Filtrado de tráfico, bloqueo de publicidad y protección frente a rastreos.
- Administración de Sistemas Operativos (ASO) → Supervisión de servicios de red.


##	Inteligencia artificial (Local / API)
- Implantación de Aplicaciones Web (IAW) → Integración de servicios web y APIs.
- Administración de Sistemas Operativos (ASO) → Gestión de dependencias y optimización de recursos.
- Seguridad y Alta Disponibilidad (SAD) → Control del acceso a modelos locales y seguridad en el procesamiento de datos.


##	Zoho mail
- Servicios de Red e Internet (SRI) → Configuración de servicios de correo con dominio propio.
- Planificación y Administración de Redes (PAR) → Configuración de registros MX, SPF, DKIM y DMARC.
- Seguridad y Alta Disponibilidad (SAD) → Protección frente a spam y uso de autenticación segura.
  

## Netdata
- Seguridad y Alta Disponibilidad (SAD) → Monitorización en tiempo real del rendimiento del sistema.
- Administración de Sistemas Operativos (ASO) → Detección de cuellos de botella y gestión de procesos.


## GitHub y Github pages
	
- Implantación de Aplicaciones Web (IAW) → Control de versiones y despliegue continuo, alojamiento de documentación y hosting de la web del proyecto.
- Empresa e Iniciativa Emprendedora (EIE) → Presentación profesional de proyectos y portafolios, colaboración y publicación de proyectos tecnológicos.
- Servicios de Red e Internet (SRI) → Publicación de contenidos mediante HTML/CSS/JS.


## Conclusión

El ecosistema bitCLD no solo integra servicios modernos y reales, sino que reproduce de manera práctica todos los ámbitos formativos del ciclo ASIR, abarcando desde la administración de sistemas operativos hasta la seguridad, redes, y despliegue de aplicaciones web.
Cada componente implementado actúa como un laboratorio funcional que refleja los objetivos de aprendizaje del ciclo, con un enfoque realista y aplicable al entorno profesional.