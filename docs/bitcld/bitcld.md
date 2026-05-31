<p align="center">
  <img src="../../assets/img/general/logonobg.png" alt="Logo bitCLD" width="400">
</p>

# Ecosistema Autoalojado de Servicios en la Nube

## Breve descripción del proyecto 

Nuestro proyecto consiste en una infraestructura de servicios auto alojados ofrecidos como servicios en la nube. BitCLD, mediante contenedores docker ofrece servicios de productividad (almacenamiento, gestor de contraseñas, servidor DNS, gestor de contenedores), inteligencia artificial (mediante API y modelos locales), monitorización (tanto de servicios como del propio servidor) y seguridad perimetral (gracias al reverse proxy). 

También hemos añadido una VPN para un acceso remoto seguro a la terminal de sistema y como backup para acceder a los servicios y la gestión de un correo profesional con nuestro dominio personalizado. Con este proyecto no necesitamos abrir puertos del servidor a internet, ofreciéndonos aspectos de ciberseguridad y buenas prácticas. 
		

## Servicios Incluidos

| Técnologías usadas | Función |
|----------|---------|
| Ubuntu Server  | En su versión 24.04.2 como sistema operativo de nuestro servidor  |
| Docker y docker compose  | Para ofrecer nuestros servicios mediante contenedores |
| Tailscale  | Como solución VPN |
| Cloudflare  | Como gestor de DNS, Cloudflared Tunnel, Cloudflare Access, gestor de dominio y subdominios, Cloudflare Pages |
| Caddy server | Como reverse proxy |
| Dodge | Como gestor de contenedores |
| Adguard Home | Como solución de filtrado DNS para mejorar la privacidad y seguridad. |
| Nextcloud  | Como un sistema de almacenamiento seguro y privado en la nube |
| Inteligencia artificial| Implementar inteligencia artificial tanto en local como mediante APIs externas. |
| Vaultwarden  | Como gestor de contraseñas seguro y accesible |
| Uptime Kuma | Para la monitorización y alertas de servicios mediante Telegram |
| Netdata  | Para monitorizar en tiempo real nuestro mini servidor |
| Zoho mail | Para la creación de un correo profesional usando nuestro dominio personalizado |
| GitHub y Github pages | Para tener una copia de seguridad del proyecto y para desplegar la página web de [bitcld.com](https://bitcld.com/)  |


## Resultados obtenidos

Tras finalizar el proyecto disponemos de una infraestructura completa y un gran aprendizaje sobre redes, sistemas y ciberseguridad. Hemos podido aprender acerca del despliegue de aplicaciones en contenedores y su gestión. El aprendizaje y uso de una plataforma tan avanzada como lo es Cloudflare, donde a parte de gestionar el dominio, también hemos trabajados con redes mediantes los túneles y ciberseguridad con la autorización de usuarios mediante Access. 

También hemos podido experimentar con la inteligencia artificial tanto en modelos locales (en los cuáles no tuvimos muy buenos resultados debido a las limitaciones de hardware de nuestro servidor) y mediante una API gratuita de Groq, con la que obtuvimos mejores resultados ya que no dependemos del procesamiento de nuestra máquina. 

Hemos tenido la oportunidad de trabajar también, relacionado con la seguridad, con una VPN principalmente para acceso remoto a la consola de comandos de nuestro sistema. Y finalmente también gracias al desarrollo de nuestra web principal hemos tenido la oportunidad de trabajar con tecnologías como GitHub y GitHub Pages. Adicionalmente, para poder tocar todas las opciones posibles, la documentación versión web del proyecto usamos Markdown, Material y Cloudflare Pages. 

En definitiva hemos trabajado con diversas tecnologías y aprendido enormemente gracias al proyecto y como resultado hemos obtenido un ecosistema de herramientas que nos ofrecen un valor real con posibilidad de usarlo en nuestro día a día.


