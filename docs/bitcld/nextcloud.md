# Nextcloud

## Introducción

Nextcloud es una plataforma de código abierto tanto de almacenamiento como de sincronización y colaboración en la nube, está diseñada para darnos la oportunidad de tener una alternativa privada y auto alojada a servicios como Google Drive o Dropbox. Nos permite alojar, gestionar y compartir archivos, calendarios, contactos, y añadir más aplicaciones como mapas, multimedia.. Todo bajo control total del propio servidor.

## Motivo de elección

Elegimos Nextcloud como solución de almacenamiento debido sobretodo a que es de código abierto, su extensa documentación y a la posibilidad de desplegarlo mediante docker compose de forma nativa, lo cuál nos facilita enormemente la gestión gracias a Dockge y se mantiene en sincronía con todo el ecosistema. Además, Nextcloud es un ecosistema en sí mismo ya que nos ofrece la posibilidad de añadir muchas más funcionalidades como calendarios, notas, edición colaborativa, mapas... Lo cuál es un añadido de valor a bitCLD. Otro aspecto por el cuál nos decidimos fue por su compatibilidad multiplataforma, ya que existe tanto una aplicación oficial para móvil, como para escritorio con la opción de sincronización automática, esto nos facilita enormemente su uso.

Es por todo ello que cumple con los pilares de nuestro proyecto: simplicidad, seguridad, autohospedaje y código abierto.


## Función dentro del proyecto

Dentro de bitcld, Nextcloud cumple principalmente la función de nube personal para almacenar archivos y datos sensibles o vitales. Desgracidamente debido a nuestras limitaciones técnicas actuales no podemos hacer uso de otras funciones como los mapas o el trabajos colaborativos. Sin embargo, nos sirve para poder ver las enormes posibilidades que nos permite y familiarizarnos con la tecnología. Además, en estos momentos para este proyecto sólo necesitamos hacer uso del almacenamiento y gracias a Cloudflare tunnel y Access podemos tener acceso de forma remota y segura, al igual que con la aplicación móvil usando Tailscale. 

Gracias a esto Nextcloud se convierte en nuestra nube personal privada, segura y autohosteada donde mantenemos el control de todos nuestros datos.


## Instalación

Nextcloud lo despliegamos fácilmente como contenedor Docker y lo gestionamos desde Dockge, manteniendo la coherencia y modularidad del ecosistema bitCLD.

Desde la interfaz de Dockge, creamos una nueva pila (stack) llamada nextcloud y añadimos el siguiente contenido en su archivo docker-compose.yml:

    services:
    app:
        image: nextcloud:latest
        container_name: nextcloud-app
        restart: unless-stopped
        ports:
        - 8080:80
        volumes:
        - /srv/docker/stacks/nextcloud/config:/var/www/html/config 
        - /mnt/dexterno/nextcloud/data:/var/www/html/data # data en disco externo
        depends_on:
        - db

    db:
        image: mariadb:10.8
        container_name: nextcloud-db
        restart: unless-stopped
        command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
        environment:
        - MYSQL_ROOT_PASSWORD=<contraseña segura>
        - MYSQL_DATABASE=<nombre de la base de datos>
        - MYSQL_USER=<usario de la base de datos>
        - MYSQL_PASSWORD=<contraseña segura>
        volumes:
        - /srv/docker/stacks/nextcloud/db:/var/lib/mysql


#### Explicación del docker compose

    app:
        image: nextcloud:latest
        container_name: nextcloud-app
        restart: unless-stopped

| Parámetro | Descripción | 
|----------|---------|
| `image: nextcloud:latest` | Usa la última versión oficial de la imagen de Nextcloud desde Docker Hub. | 
| `container_name: nextcloud-app` | Asigna un nombre fijo al contenedor (útil para comandos y depuración). | 
| `restart: unless-stopped` | Hace que el contenedor se reinicie automáticamente si se detiene de forma inesperada, salvo que se haya detenido manualmente. | 

        ports:
        - 8080:80
| Puertos expuestos | Descripción | 
|----------|---------|
| `ports: - 8080:80` | Expone el puerto 80 del contenedor y accederemos a su interfaz desde el puerto 8080. | 
 

        volumes:
        - /srv/docker/stacks/nextcloud/config:/var/www/html/config 
        - /mnt/dexterno/nextcloud/data:/var/www/html/data # data en disco externo
         
| Volumen | Descripción | 
|----------|---------|
| `- /srv/docker/stacks/nextcloud/config:/var/www/html/config ` | Carpeta de configuración del contenedor localizada en el almacenamiento interno | 
| `- /mnt/dexterno/nextcloud/data:/var/www/html/data` | Carpeta de almacenamiento de nuestras fotos y archivos en el disco duro externo | 

        depends_on:
        - db

| Parámetro | Descripción | 
|----------|---------|
|`depends_on: - db` | Indica que no inicie el servicio de Nextcloud sin antes que antes se haya iniciado la base da datos para evitar errores |

    db:
        image: mariadb:10.8
        container_name: nextcloud-db
        restart: unless-stopped

| Parámetro | Descripción | 
|----------|---------|
| `image: mariadb:10.8` | Usa la versión 10.8 de la imagen de MariaDB desde Docker Hub. No usamos la última versión ya que una actualización nos podría romper algo. | 
| `container_name: nextcloud-db` | Asigna un nombre fijo al contenedor (útil para comandos y depuración). | 
| `restart: unless-stopped` | Hace que el contenedor se reinicie automáticamente si se detiene de forma inesperada, salvo que se haya detenido manualmente. | 

command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW

>Es esencial para ajustar MariaDB a Nextcloud. 

| Parámetro | Descripción | 
|----------|---------|
| ` --transaction-isolation=READ-COMMITTED ` | Gestiona cómo se bloquean los datos cuando se leen. Evita que la base de datos se bloquee cuando hay muchas peticiones a la vez. | 
| ` --binlog-format=ROW ` | Formato de registro binario. Necesario para funciones avanzadas y estabilidad de Nextcloud. Sin esta línea, Nextcloud te daría advertencias o errores graves. | 

        environment:
        - MYSQL_ROOT_PASSWORD=contraseña
        - MYSQL_DATABASE=nextcloud
        - MYSQL_USER=nextcloud
        - MYSQL_PASSWORD=contraseña

> Al arrancar por primera vez, el contenedor lee esto y crea automáticamente la base de datos nextcloud, el usuario nextcloud y les asigna la contraseña. Por razones de seguridad no compartiremos los datos reales:

| Environment | Descripción | 
|----------|---------|
| `- MYSQL_ROOT_PASSWORD=superseguro` | En este campo indicamos la contraseña root de nuestra base de datos |
| `- MYSQL_DATABASE=nextcloud` | En este campo indicamos el nombre de nuestra base de datos |
| `- MYSQL_USER=nextcloud` | En este campo indicamos el nombre de usuario en nuestra base de datos |
| `- MYSQL_PASSWORD=contraseña` | En este campo indicamos la contraseña de nuestra base de datos |

        volumes:
        - /srv/docker/stacks/nextcloud/db:/var/lib/mysql
| Volumes | Descripción | 
|----------|---------|
| `- /srv/docker/stacks/nextcloud/db:/var/lib/mysql` | Persistencia de la base de datos.|

> Es necesario que este volumen esté en el SSD interno y no en el disco duro externo ya que Las bases de datos necesitan hacer muchas operaciones pequeñas y rápidas. Por lo que si lo ponemos en el disco externo, Nextcloud nos irá lentísimo.

## Configuración

Una vez lo hayamos desplegado e iniciado desde Dockge podremos acceder a `http://192.168.1.5:8080` para configuarlo.

### 1. Creamos cuenta de administrador
Para empezar deberemos crear una cuenta de administrador de Nextcloud en la que tendremos que poner un nombre de usuario y una contraseña

<p align="center">
  <img src="../../assets/img/nextcloud/admin.png" alt="Crear Cuenta Nextcloud" width="300">
</p>

Por defecto tendremos como base de datos a SQLite, la cuál incluso Nextcloud nos la desaconseja ya que es muy limitada. Por lo que le daremos a la opción de Storage & database y seleccionaremos la opción MySQL/MariaDB

<p align="center">
  <img src="../../assets/img/nextcloud/admin3.png" alt="Crear Cuenta Nextcloud" width="250">
</p>

Y añadiremos los datos que hemos puesto anteriormente en el docker compose en db

<p align="center">
  <img src="../../assets/img/nextcloud/admin2.png" alt="Crear Cuenta Nextcloud" width="250">
</p>

En la opción de LocalHost deberemos añadir db ya que es el nombre que le hemos puesto a nuestro servicio.

En la opción de añadir aplicaciones en nuestro caso, al ser usada sólo como almacenamiento, seleccionaremos contacs y calendar.

<p align="center">
  <img src="../../assets/img/nextcloud/recomendada.png" alt="Crear Cuenta Nextcloud" width="400">
</p>

Y ya podremos acceder a nuestro Nextcloud y guardar nuestros archivos

<p align="center">
  <img src="../../assets/img/nextcloud/dentro.png" alt="Crear Cuenta Nextcloud" width="700">
</p>

### 2. Ajustar config.php

Sin embargo, aunque tengamos configurado caddy o cloudflare no podremos acceder a Nextcloud mediante `cld.lan` o `cld.bitcld.com`:

<p align="center">
  <img src="../../assets/img/nextcloud/dominiono.png" alt="config.php" width="600">
</p>

Para solucionar esto deberemos editar el archivo config.php:

        sudo nano /srv/docker/stacks/nextcloud/config/config.php

Y dentro del archivo buscaremos `'trusted_domains' array` y añadiremos los dominios desde donde accederemos a Nextcloud:

<p align="center">
  <img src="../../assets/img/nextcloud/trusted.png" alt="config.php" width="600">
</p>

De esta manera ya podremos acceder desde `cld.lan` o `cld.bitcld.com`:

<p align="center">
  <img src="../../assets/img/nextcloud/dominiosi.png" alt="config.php" width="600">
</p>


### Aplicación para móviles

Para que podamos usar la aplicación móvil es muy simple pero deberemos tener cuenta factores de serguridad: 

Existen dos maneras de hacerlo añadiendo la ruta a nuestro dominio `cld.bitcld.com` o a la ip que nos ofrece Tailscale. Ambos tienen sus ventajas y desventajas.

#### cld.bitcld.com

Si queremos usar nuestro dominio personalizado como ruta nos ofrece la ventaja que no deberemos tener que estar conectados a Tailscale cada vez que queramos acceder o subir algún archivo. Sin embargo, esta opción nos obligaría a quitar Access para la aplicación de nextcloud o añadir una regla de Bypass para la aplicación lo cuál es más complejo.
> Es necesario hacer una de las dos opciones ya que con Access activado la aplicación no sabe como gestionar la pantalla extra de verificación de email y código. Por lo que nos da error.

#### Tailscale

La otra opción es usar la ip que nos ofrece Tailscale. Al usar esta opción no tendremos que modificar nada en el Cloudflare Access y es más simple. La principal desventaja como hemos comentado, es que tendremos que estar siempre conectados a la VPN para acceder a la app.

#### Decisión

Nosotros por comodidad y simplicidad hemos decidido usar la opción de Tailscale, para así poder seguir teniendo la aplicación de nextcloud protegida con Access o no tener que estar modificando las reglas. Ya que para nuestro uso no es un inconveniente activar la VPN cuando vayamos a usar la aplicación. 

#### Configuración de la APP

Para configurar la aplicación mediante Tailscale:

1. Buscaremos la ip de nuestro servidor que nos da Tailscale

    <p align="center">
    <img src="../../assets/img/nextcloud/movil2.jpg" alt="config.php" width="400">
    </p>

2. Añadiremos la ruta con la ip con el puerto 8080 de Tailscale en el archivo `/srv/docker/stacks/nextcloud/config/config.php`
   
    <p align="center">
    <img src="../../assets/img/nextcloud/movil1.png" alt="config.php" width="400">
    </p>

3. Conectados a la VPN pondremos la ruta en la aplicación

    <p align="center">
    <img src="../../assets/img/nextcloud/movil3.jpg" alt="config.php" width="300">
    </p>

4. Para poder sincronizarla se abrirá una pestaña en nuestro navegador y deberemos iniciar sesión 

    <p align="center">
    <img src="../../assets/img/nextcloud/movil4.jpg" alt="config.php" width="300">
    </p>

5. Una vez hecho el inicio de sesión la cuenta se conectará

    <p align="center">
    <img src="../../assets/img/nextcloud/movil5.jpg" alt="config.php" width="300">
    </p>

Y ya podremos acceder a Nextcloud desde nuestra app

<p align="center">
<img src="../../assets/img/nextcloud/movil6.jpg" alt="config.php" width="300">
</p>


Además también podemos añadir muchas más aplicaciones para completar nuestra suit con Nextcloud, existen aplicaciones de mapas, de multimedia, para colaboración de documentos... Desgracidamente con nuestro servidor actual estamos muy limitados y no podemos acceder más funciones que nos ofrece Nextcloud.

<p align="center">
<img src="../../assets/img/nextcloud/uso1.png" alt="config.php" width="700">
</p>


## Conclusión

Nextcloud, en bitCLD, es nuestra solución para poder tener un almacenamiento personal, privado y seguro. Podemos acceder a través del subdominio cld.bitcld.com y cld.lan y es gestionada mediante Dockge en su propio contenedor docker-compose. Gracias a Nextcloud podemos disponer de una nube propia sin depender de otros servicios externos a nuestro control, lo que nos garantiza un control total de nuestros datos en nuestro disco duro externo que puede ser ampliable.

Gracias a nuestra solución de VPN (Tailscale), podemos usar la aplicación oficial de Nextcloud en el móvil para sincronizar y acceder a todos nuestros archivos de forma inmediata y cifrada, con una experiencia idéntica a otras nubes como Google Drive o Outlook pero sin necesidad de abrir puertos en nuestro servidor. Con nuestra configuración, el servicio nos ofrece un almacenamiento seguro con alta disponiblidad sin añadir más complejidad innecesaria haciéndolo lo más simple posible.