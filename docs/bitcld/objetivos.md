# Objetivos generales y específicos

## 1. Objetivos generales
Estos son los objetivos generales que nos hemos planteado para nuestro proyecto final.

### 1.1 Servir como proyecto final de ASIR
Principalmente debe servirnos como proyecto final de ASIR por lo que debemos tocar todos los módulos posibles para demostrar nuestro conocimiento en todas las áreas que podamos como redes, seguridad, virtualización, gestión de dominios… Además, también debe lucir lo más profesional posible para poder añadirlo a nuestro CV.
### 1.2 Crear un ecosistema lo más completo y seguro posible
Para ello creamos deberemos una infraestructura, basada principalmente en contenedores Docker, que nos ofrezcan servicios de distintos tipos pero que a su vez la hagamos de la manera más segura posible.
### 1.3 Crear un entorno seguro
Buscamos también que bitCLD pueda ser un entorno seguro para tener un lugar donde poder trabajar cómodamente para hacer todas las pruebas que queramos al estilo un laboratorio personal para desplegar servicios.
### 1.4 Crear una arquitectura escalable
Nuestro ecosistema debe tener la oportunidad de poder ser fácilmente ampliado para poder seguir aprendiendo y creciendo profesionalmente.

## 2. Objetivos específicos

Para lograr los objetivos generales hemos establecido estos objetivos específicos.

### 2.1 Configuración de servidores
Implementaremos un servidor físico con Ubuntu Server haciendo todas las configuraciones necesarias para que esté lo más optimizado y seguro posible.
### 2.2 Gestión de contenedores
Para poder hacerlo lo más escalable posible desplegaremos nuestros servicios, siempre que sea posible, desde contenedores docker compose gracias a la escalabilidad que nos ofrece y a la gestión tan fácil y cómoda que tenemos con Dockge.
### 2.3 Seguridad con Cloudflare
Para que podamos acceder de manera segura a nuestros servicios de manera segura sin abrir puertos gracias a Cloudflare Tunnel. Además Cloudflare también se encarga de gestionar nuestro dominio y subdominios, de autenticar usuarios mediante Access y proteger nuestras aplicaciones y página web.
### 2.4 Proxy inverso
Para una mayor seguridad usaremos Caddy server como proxy inverso en nuestra red local con certificados SSL y para que nuestros servicios sean más cómodos de buscar en nuestra red LAN.
### 2.5 Acceso a una VPN
Usaremos Tailscale para poder trabajar con una red privada virtual y para usarla como un acceso externo secundario a nuestro sistema en caso de necesitar acceder a la terminal en algún momento.
### 2.6 Monitorización de servicios
Implementaremos Uptime kuma para monitorizar nuestros servicios y para que podamos recibir alertas en caso de algún imprevisto.
### 2.7 Almacenamiento en la nube
Añadiremos Nextcloud para usarlo como sistema de almacenamiento privado y seguro en la nube, además de notas y calendario. Debido a las características de nuestro proyecto una herramienta como esta era necesaria.
### 2.8 Gestión de contraseñas
Como gestor de contraseñas seguras usaremos Vaultwarden para poder tener todas las contraseñas que necesitemos a buen recaudo y también para tener buenas prácticas ya que lo más seguro es nunca repetir contraseña.
### 2.9 Filtrado de DNS
Usaremos Adguard Home como nuestro servidor de DNS filtrado para nuestra red. Además, lo personalizaremos mediante listas de bloqueo previamente seleccionadas que nos bloquean tanto los  anuncios y el malware.
### 2.10 Inteligencia artificial
Debido a la importancia de esta, trabajaremos con la inteligencia artificial tanto de manera local en nuestro servidor como mediante API externa.
### 2.11 Monitorización de rendimiento
Usarmos Netdata para monitorizar en tiempo real nuestro sistema, y en caso de ser necesario recibir notificaciones de cualquier problema que pueda haber en nuestro equipo
### 2.12 Gestión de código
Usaremos GitHub para poder gestionar la página web y nuestra documentación profesional con Mkdocs y material y desplegado mediante Cloudflare Pages. Además de ser públicos para que cualquiera pueda verlo y usarlo.
### 2.13 Crear una imagen profesional
Intentamos crear una identidad propia y profesional al proyecto creando un logo y usando una paleta de colores coherente en todo momento. Además, de gestionar nuestro propio dominio bitcld.com para que sea el eje principal del proyecto, alojando tanto la web como las aplicaciones y documentación en él.
### 2.14 Cumplir objetivos académicos
Buscamos poder completar mi ciclo formativo de grado superior y demostrar nuestros conocimientos en todos los módulos posibles de ASIR como Implantación de Sistemas Operativos, Administración de Sistemas Operativos, Planificación y Administración de Redes, Seguridad y Alta Disponibilidad, Servicios de Red e Internet… Además de presentar una documentación completa con la que pueda demostrar que tengo esos conocimientos.
### 2.15 Cumplir objetivos profesionales
A nivel profesional buscábamos crear un proyecto lo más completo posible para demostrar nuestras capacidades a posibles empleadores ya que un proyecto profesional y completo puede abrir puertas.


