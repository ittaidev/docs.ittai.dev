# Vaultwarden

## Introducción

Vaultwarden es una implementación ligera y autohosteada del gestor de contraseñas de Bitwarden que fue desarrollada por Rust y que ofrece las mismas funcionalidades sin depender de ningún servidor externo. Permite almacenar y compartir contraseñas de forma segura con cifrado de extremo a extremo. Y debido a que tiene compatibilidad nos proporciona una experiencia igual.

## Motivos de elección

Elegimos vaultwarden debido a la necesidad de almacenar nuestras credenciales de manera cifrada con AES-256 y al ser una solución ligera, eficiente y privada al ser autogestionada. Además, algo esencial para nuestro proyecto es que podamos desplegarlo en un contenedor. Todo esto sumado a que es un software libre nos ofrece control total y está por lo que se alinea a los pilares de nuestro proyecto.

## Función dentro del proyecto

Dentro de bitCLD, como es de esperar, actúa como nuestro gestor de contraseñas seguras para nuestras aplicaciones. Como nuestros otros servicios el acceso lo hacemos en local mediante pass.lan y a través de nuestro subdominio pass.bitcld.com. Debido a la importancia de lo que se almacena, no existe una ruta con puerto en nuestra red local. Esta aplicación ofrece seguridad al proyecto y nos muestra la importancia de usar gestores de contraseñas en nuestro día a día.

## Instalación

Vaultwarden se despliega fácilmente como contenedor Docker y se gestiona desde Dockge, manteniendo la coherencia y modularidad del ecosistema bitCLD.

Desde la interfaz de Dockge, creamos una nueva pila (stack) llamada vaultwarden y añadimos el siguiente contenido en su archivo docker-compose.yml:

        services:
        vaultwarden:
            image: vaultwarden/server:latest
            container_name: vaultwarden
            restart: unless-stopped
            environment:
            SIGNUPS_ALLOWED: "false"
            INVITATIONS_ALLOWED: "false"
            ADMIN_TOKEN: <MiToken>
            ROCKET_PORT: "80"
            volumes:
            - ./data:/data
            networks:
            caddy_net:
                ipv4_address: 172.28.0.2
            expose:
            - "80"
        networks:
        caddy_net:
            external: true

### Explicación del docker compose

        image: vaultwarden/server:latest
        container_name: vaultwarden
        restart: unless-stopped
| Parámetro | Descripción | 
|----------|---------|
| `image: vaultwarden/server:latest` | Usa la última versión oficial de la imagen de Vaultwarden disponible desde Docker Hub. | 
| `container_name: vaultwarden` | Asigna un nombre fijo al contenedor (útil para comandos y depuración). | 
| `restart: unless-stopped` | Hace que el contenedor se reinicie automáticamente si se detiene de forma inesperada, salvo que se haya detenido manualmente. | 

        environment:
        SIGNUPS_ALLOWED: "no"
        INVITATIONS_ALLOWED: "false"
        ADMIN_TOKEN: <MiToken>
        ROCKET_PORT: "80"
| Environment | Descripción | 
|----------|---------|
| `SIGNUPS_ALLOWED: "no"` | Usar "yes" solo al principio para crear el primer usuario; después debe cambiarse a "no" por seguridad. | 
| `INVITATIONS_ALLOWED: "false"` | Controla si se pueden enviar invitaciones a otros usuarios desde el panel de administración. Al estar en "false", solo los administradores pueden crear nuevas cuentas manualmente. | 
| `ADMIN_TOKEN: <MiToken>` | Token secreto que permite acceder al panel de administración en /admin. Debe ser una cadena larga y única (por ejemplo, generada con openssl rand -hex 32).| 
| `ROCKET_PORT: "80"` | Indica el puerto interno en el que Vaultwarden escucha dentro del contenedor. Caddy y Cloudflare Tunnel se conectarán a este puerto. | 

        volumes:
        - ./data:/data
         
| Volumen | Descripción | 
|----------|---------|
| `- ./data:/data` | Crea un volumen persistente para los datos de Vaultwarden. Todo lo que guarda (base de datos SQLite, archivos cifrados, configuración) queda dentro de ./data en mi servidor, asegurando que la información no se pierda aunque el contenedor se borre o actualice. | 

            networks:
            caddy_net:
                ipv4_address: 172.28.0.2

| Red | Descripción | 
|----------|---------|
|`caddy_net` | El servicio se conecta a la red caddy_net, lo que permite que Caddy se comunique con este contenedor |
|`ipv4_address: 172.28.0.2` | Al contenedor se le asigna una IP fija |

> La IP fija nos garantiza que el servicio sea accesible desde Caddy o Cloudflare Tunnel siempre en la misma dirección. También facilita su monitoreo con Uptime Kuma, ya que el destino no cambia.

    expose:
      - "80"

| Parámetro | Descripción | 
|----------|---------|
|`expose: - "80"` | “Expone” el puerto 80 dentro de la red interna de Docker, para que otros contenedores (como Caddy o Cloudflare Tunnel) puedan comunicarse con Vaultwarden. No lo publica hacia el exterior, solo dentro de la red. |

        networks:
        caddy_net:
            external: true

| Red | Descripción | 
|----------|---------|
|`external: true` | Indica que la red caddy_net ya existe y no será creada por este docker-compose. Esto permite compartir la misma red entre distintos docker-compose.yml, haciendo posible que varios contenedores se comuniquen entre sí. |

[Dockge](./dockge.md) se encargará de crear automáticamente la carpeta data dentro del directorio de la pila para almacenar la configuración y el historial de monitorización de forma persistente.

Una vez guardado, basta con darle al botón de desplegar para que el servicio se inicie.

## Configuración

Para poder crear una cuenta y usar Vaultwarden debemos asegurarnos que arrancamos el docker compose con registro habilitado (solo para crear la primera cuenta). 

        environment:
        SIGNUPS_ALLOWED: "yes"
        INVITATIONS_ALLOWED: "false"
        ADMIN_TOKEN: Mi Token
        ROCKET_PORT: "80"

> Una vez esté creada ya sólo tendremos que establer ´SIGNUPS_ALLOWED: en "false" por seguridad, para que no se pueda crear más cuentas.

1. La primera vez que lo arrancamos, una vez configurado Caddy y Cloudflare Access, tendremos que dirigirnos a `pass.bitcld.com` o a `pass.lan` y dar a la opción de crear cuenta. Dónde pondremos un e-mail, un nombre de usuario y una contraseña maestra (la cuál debe ser lo más segura posible):

    <p align="center">
    <img src="../../assets/img/vault/crear-cuenta.png" alt="Creación cuenta Vaultwarden" width="900">
    </p>
    <p align="center">
    <img src="../../assets/img/vault/password.png" alt="Creación cuenta Vaultwarden" width="900">
    </p>

2. Una vez creada la cuenta lo siguiente sería detener el contenedor y cambiar la opción de `SIGNUPS_ALLOWED:` a `false`.
3. En caso de necesitar eleminar alguna cuenta, o ir al panel de administración deberemos abrir http://pass.bitcld.com/admin
e introducir el ADMIN_TOKEN para entrar al panel de administración.

<p align="center">
<img src="../../assets/img/vault/admin.png" alt="Panel de administración" width="900">
</p>
<p align="center">
<img src="../../assets/img/vault/admin2.png" alt="Panel de administración" width="900">
</p>

### Creación de inicio de sesión 
Para la creación de inicio de sesión es muy sencillo:

1. Seleccionamos Nuevo > Inicio de sesión
   
    <p align="center">
    <img src="../../assets/img/vault/nuevo-inicio.png" alt="Creación cuenta Vaultwarden" width="300">
    </p>
2.  Elegimos los parámetros que queremos que en incluyan en  nuestra nueva contraseña
    <p align="center">
    <img src="../../assets/img/vault/nuevo-inicio2.png" alt="Creación cuenta Vaultwarden" width="400">
    </p>
3.  Introducimos y los datos que consideremos necesarios (para una mejor organización se recomienda el uso de carpetas).
    <p align="center">
    <img src="../../assets/img/vault/nuevo-inicio3.png" alt="Creación cuenta Vaultwarden" width="500">
    </p>
4. Ya lo tendríamos creado
    <p align="center">
    <img src="../../assets/img/vault/nuevo-inicio4.png" alt="Creación cuenta Vaultwarden" width="700">
    </p>

## Seguridad
El acceso a nuestro gestor de contraseñas se realiza exclusivamente a través de Tailscale VPN o mediante un Cloudflare Tunnel protegido por Access, disponible externamente en el subdominio
`pass.bitcld.com` y localmente en `pass.lan`. De esta forma, el servicio permanece completamente aislado del exterior y solo accesible por los medios autorizados dentro del ecosistema bitCLD.

> Para más información sobre la seguridad del ecosistema accede a la documentación de [Cloudflare](./cloudflare.md), [Tailscale](./tailscale.md) o [Caddy](./caddy.md).

## Conclusión

Vaultwarden es una aplicación fundamental en bitCLD ya que nos permite gestionar todas nuestras contraseñas de una manera muy simple y segura. El que sólo exista la opción de acceder de manera cifrada https:// nos garantiza mucha más seguridad evitando posibles accesos no autorizados. Con todo esto obtenemos más fiabilidad, seguridad y privacidad sobre nuestras credenciales.