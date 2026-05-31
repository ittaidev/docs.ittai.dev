# Uptime Kuma

## Introducción

Uptime Kuma es una herramienta autoalojada que monitoriza servicios para comprobar su disponibilidad y su rendimiento. Dispone de una interfaz muy intuitiva y pertenece al mismo equipo que desarrolla Dockge. Y también se trata de una solución de código abierto.

## Motivos de elección

Elegimos Uptime Kuma por su simplicidad, su facilidad de administración y gestión y sobretodo porque es una solución de código abierto y se puede desplegar mediante Docker, lo cuál lo hace ideal para nosotros. Además, de ser muy ligero también nos permite recibir notificaciones en Telegram, correo electrónico, discord... Debido a todo esto fue la aplicación elegida para la monitorización ya que cumple los pilares de nuestro proyecto: simple, seguro, autogestionado y de código abierto.

## Función dentro del proyecto

Dentro de bitCLD, Uptime Kuma se encarga de monitorizar cada 60 segundos la disponibilidad de cada uno de nuestros servicios y también nos envía alertas en nuestro canal de Telegram dedicado para el proyecto indicándonos si está desplegado o no el servicio.

## Instalación

Uptime Kuma se despliega fácilmente como contenedor Docker y se gestiona desde Dockge, manteniendo la coherencia y modularidad del ecosistema bitCLD.

Desde la interfaz de Dockge, creamos una nueva pila (stack) llamada uptime-kuma y añadimos el siguiente contenido en su archivo docker-compose.yml:

        services:
        uptime-kuma:
            image: louislam/uptime-kuma:latest
            container_name: uptime-kuma
            restart: unless-stopped
            volumes:
            - /srv/docker/stacks/uptime-kuma:/app/data
            ports:
            - 3001:3001
            networks:
            - caddy_net
        networks:
        caddy_net:
            external: true



Dockge se encargará de crear automáticamente la carpeta data dentro del directorio de la pila para almacenar la configuración y el historial de monitorización de forma persistente.

Una vez guardado, basta con darle al botón de desplegar para que el servicio se despliegue.

#### Explicación del docker compose

            image: louislam/uptime-kuma:latest
            container_name: uptime-kuma
            restart: unless-stopped
| Parámetro | Descripción | 
|----------|---------|
| `image: louislam/uptime-kuma:latest` | Usa la última versión oficial de la imagen de Uptime Kuma disponible desde Docker Hub. | 
| `container_name: uptime-kuma` | Asigna un nombre fijo al contenedor (útil para comandos y depuración). | 
| `restart: unless-stopped` | Hace que el contenedor se reinicie automáticamente si se detiene de forma inesperada, salvo que se haya detenido manualmente. | 

          volumes:
          - /srv/docker/stacks/uptime-kuma:/app/data

| Volumen | Descripción | 
|----------|---------| 
| `volumes: - /srv/docker/stacks/uptime-kuma:/app/data` | Guarda la configuración interna y datos persistentes de Uptime Kuma (así no se pierden lass configuraciones). | 

            ports:
            - 3001:3001
         
| Puerto | Descripción | 
|----------|---------|
| `ports: - 3001:3001` | Puerto del host que se usará en el navegador, Cloudflare y Caddy |

        networks:
        caddy_net:
            external: true

| Red | Descripción | 
|----------|---------|
|`caddy_net` | El servicio se conecta a la red caddy_net, lo que permite que Caddy se comunique con otros contenedores |
|`external: true` | Indica que la red caddy_net ya existe y no será creada por este docker-compose. Esto permite compartir la misma red entre distintos docker-compose.yml, haciendo posible que varios contenedores se comuniquen entre sí. |

## Configuración

Para que podamos acceder al panel web de administración debemos irnos a esta dirección en nuestro navegador: `http://192.168.1.5:3001`

En el primer acceso se solicitará crear un usuario administrador. Creamos un usuario y una contraseña y ya podremos acceder al menú de configuración

<p align="center">
  <img src="../../assets/img/kuma/panel2.png" alt="Panel Uptime-kuma" width="900">
</p>

### Creación de un nuevo monitor

Para la creación de nuevo monitor para un servicio es muy sencillo, simplemente debemos establecer el tipo, en nuestro caso HTTP(s), un nombre,  la URL `http://192.168.1.5:puerto` (en nuestro caso utilizamos el puerto y no el enlace `.lan` para poder seguir comprobando nuestros servicios sin necesidad de que Caddy esté desplegado).

<p align="center">
  <img src="../../assets/img/kuma/crear-monitor.png" alt="Panel Uptime-kuma" width="600">
</p>

Una vez guardado ya podremos ver el estado de nuestro servicio en todo momento tanto si está activo o si ha caído.

<p align="center">
  <img src="../../assets/img/kuma/estado.png" alt="Estado servicio" width="600">
</p>


### Configuración sin puerto establecido en el docker compose

En caso de no habilitar ningún puerto, como es nuestro caso con Vaultwarden, deberemos asegurarnos que Uptime Kuma esté en la misma red que Vaultwarden y Caddy y en la sección de URL ponemos el nombre del docker compose correspondiente y el que hayamos establecido en el Rocket Expose, esta variable se utiliza para configurar el puerto interno del contenedor donde se ejecuta la aplicación, en nuestro caso, VaultWarden. Para ver la documentación de Vaultwarden [pulsa aquí](./vaultwarden.md/).

<p align="center">
  <img src="../../assets/img/kuma/vault.png" alt="Monitor Vaultwarden" width="400">
</p>


### Creación de notificación

Para poder recibir las notificaciones por Telegram, Discord, Email o la plataforma de nuestra elección. Debemos irnos a los monitores ya creados y en la sección de notificaciones seleccionamos configurar notificación. 

<p align="center">
  <img src="../../assets/img/kuma/noti.png" alt="Creación de bot en Telegram" width="600">
</p>

#### Creación de bot

En nuestro caso al ser mediante notificación en Telegram necesitaremos crear un bot, para ello:

1. Iniciamos un chat con @BotFather
2. Seleccionamos la opción de /newbot
3. Indicamos un nombre 
4. Indicamos un nombre de usuario para el bot
5. Ya tendríamos el bot creado

<p align="center">
  <img src="../../assets/img/kuma/nuevo-bot.png" alt="Creación de bot en Telegram" width="600">
</p>
<p align="center">
  <img src="../../assets/img/kuma/nuevo-bot2.png" alt="Creación de bot en Telegram 2" width="600">
</p>



##### Bot en una conversación

En caso de quererlo como una conversación con el bot. Una vez tengamos el bot creado necestaremos conocer nuestro ID de chat, para ello existen diversos bots que ofrecen eso. En nuestro caso elegimos el bot de @showmeidbot. Simplemente debemos iniciar una conversación con él:

<p align="center">
  <img src="../../assets/img/kuma/showme.png" alt="Estado servicio" width="600">
</p>

Con el bot creado y el ID del chat de nuestra conversación ya podremos crear la notificación en Uptime Kuma:

<p align="center">
  <img src="../../assets/img/kuma/notificacion.png" alt="Configurar notificación" width="400">
</p>

Una vez configurado el bot deberíamos recibir las notificaciones de los servicios que estamos monitorizando de manera automática:

<p align="center">
  <img src="../../assets/img/kuma/movil.png" alt="Estado de los servicios" width="600">
</p>

##### Bot en un grupo de Telegram

En caso de querer el bot en un grupo de Telegram, como será nuestro caso, ya que al tener un bot para las notificaciones de Uptime Kuma y otro para las de [Netdata](./netdata.md) por comodidad es mejor tener los dos bots en un grupo de telegram.  

En este caso la creación del bot sería la misma. 

1. Debemos crear un grupo de telegram
2. Añadimos ambos bots en nuestro grupo como administradores
  <p align="center">
  <img src="../../assets/img/kuma/administrador.png" alt="Creación de bot en Telegram" width="300">
  </p>
1. En Uptime Kuma en lugar de nuestro ID chat, necesitaremos el ID chat del grupo, por lo que añadiremos el bot al grupo para conocerlo
  <p align="center">
  <img src="../../assets/img/kuma/grupo.png" alt="Creación de bot en Telegram" width="600">
  </p>
1. Estos datos los ponemos en nuestra notificación de monitores de Uptime Kuma y ya lo tendríamos configurado
  <p align="center">
  <img src="../../assets/img/kuma/noti2.png" alt="Creación de bot en Telegram" width="300">
  </p>
  <p align="center">
  <img src="../../assets/img/kuma/grupo2.png" alt="Creación de bot en Telegram" width="650">
  </p>

> Para averiguar el ID chat tanto de la conversación o del grupo, también se puede usar el propio Uptime Kuma, enviándo un mensaje al bot o al grupo y haciendo clic en Obtener automáticamente para así no tener que usar bots externos.
  <p align="center">
  <img src="../../assets/img/kuma/extra.png" alt="Creación de bot en Telegram" width="650">
  </p>
  
## Conclusión 

Uptime Kuma es una buena aportación al ecosistema bitCLD ya que nos permite monitorizar nuestros servicios de una manera clara, ligera y mediante docker compose y gestionado por Dockge. El hecho de que podamos recibir notificaciones en Telegram nos ofrece tener una supervisión en tiempo real.
