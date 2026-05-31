# Dockge 
## Introducción

Dockge es un gestor de contenedores moderno, muy ligero y de código abierto. Nos da la oportunidad de visualizar, administrar y gestionar varios stacks de una manera centralizada y muy simple, lo que nos ahorra bastante trabajo.

## Motivos de elección

Elegimos dockge ya que necesitábamos una solución para administrar nuestros contenedores docker compose. Una de las principales razones por las que nos decidimos por este y no por otro gestor es porque nos permite editar directamente los archivos compose.yml desde su propia interfaz, a parte de crearnos las carpetas que necesitemos con sólo añadirlas al docker compose. Por todo esto lo elegimos para nuestro proyecto ya que cumple con los pilares del mismo: simple, seguro, autogestionado y de código abierto.

## Función dentro del proyecto

Dentro de bitCLD la función de Dockge es de las más importantes, ya que es el gestor de todos nuestros servicios. Desde su interfaz podemos iniciar, detener, reiniciar o editar cualquier contenedor que gestione, ahorrándonos el tener que hacerlo mediante la consola


## Instalación

### 0.1 Requisitos previos

Debemos asegurarnos de tener docker y docker compose instalado en nuestro servidor: 

        docker --version
        docker compose version

<p align="center">
  <img src="../../assets/img/dockge/previo.png" alt="Requisitos previos" width="800">
</p>

> Para ver la documentación de docker y docker compose [Pulsa el siguiente enlace](./docker.md)

Y también nos aseguramos de crear las carpetas necesarias con:

        sudo mkdir -p /srv/docker/dockge/data
        sudo mkdir -p /srv/docker/stacks


### 1. Crear el docker-compose.yml de Dockge

Creamos el archivo en `/srv/docker/dockge/docker-compose.yml` con este contenido (ajustado a nuestras necesidades):

        services:
        dockge:
            image: louislam/dockge:latest
            container_name: dockge
            restart: unless-stopped
            ports:
            - "5001:5001"
            volumes:
            - /var/run/docker.sock:/var/run/docker.sock
            - ./data:/app/data
            - /srv/docker/stacks:/srv/docker/stacks
            environment:
            - DOCKGE_STACKS_DIR=/srv/docker/stacks


#### Explicación del docker compose

            image: louislam/dockge:latest
            container_name: dockge
            restart: unless-stopped
| Parámetro | Descripción | 
|----------|---------|
| `image: louislam/dockge:latest` | Usa la última versión oficial de la imagen de Dockge disponible desde Docker Hub. | 
| `container_name: dockge` | Asigna un nombre fijo al contenedor (útil para comandos y depuración). | 
| `restart: unless-stopped` | Hace que el contenedor se reinicie automáticamente si se detiene de forma inesperada, salvo que se haya detenido manualmente. | 

            ports:
            - "5001:5001"
| Puerto | Descripción | 
|----------|---------|
| `ports: - 5001:5001` | Puerto del host que se usará en el navegador, Cloudflare y Caddy |

        volumes:
        - /var/run/docker.sock:/var/run/docker.sock
        - ./data:/app/data
        - /srv/docker/stacks:/srv/docker/stacks

| Volumen | Descripción | 
|----------|---------|
| `- /var/run/docker.sock:/var/run/docker.sock` | Es lo que permite a Dockge comunicarse directamente con Docker, listar contenedores, crear, detener, y modificar stacks. (Sin esto, Dockge no podría controlar Docker.) | 
| `- ./data:/app/data` | Guarda la configuración interna y datos persistentes de Dockge (así no se pierden lass configuraciones). | 
| `- /srv/docker/stacks:/srv/docker/stacks` | Es el directorio donde Dockge buscará, creará o modificará los docker-compose de los servicios (Nextcloud, Caddy...) Dockge creará automáticamente las carpetas necesarias al crear nuevos stacks desde su interfaz.| 

        environment:
        - DOCKGE_STACKS_DIR=/srv/docker/stacks
         
| Environment | Descripción | 
|----------|---------|
| `- DOCKGE_STACKS_DIR=/srv/docker/stacks` | indica a Dockge cuál es la ruta principal donde debe gestionar los stacks. (Aunque ya está montado como volumen, esta variable es necesaria para que la interfaz sepa dónde leer y guardar los archivos `docker-compose.yml`) |

>Un error inicial que tuve fue no ajustar el docker.sock ni el environment, por lo que no podía gestionar los docker compose de mis servicios hasta que lo ajusté bien.

### 2. Desplegar Dockge

Desde `/srv/docker/dockge`:

        docker compose pull
        docker compose up -d


Comprobamos que está en marcha:

        docker ps --filter name=dockge


<p align="center">
  <img src="../../assets/img/dockge/ps.png" alt="docker ps --filter name=dockge" width="800">
</p>


## Configuración

### Acceso a la interfaz y creación de servicios

En nuestra red local:

        http://192.168.1.5:5001

Creamos un usuario y una contraseña y ya tendremos acceso al panel de administración de Dockge.

<p align="center">
  <img src="../../assets/img/dockge/panel.png" alt="Requisitos previos" width="800">
</p>

> La primera carga  nos mostrará los stacks (vacío si aún no hay nada en /srv/docker/stacks)
 

#### Crear stack nuevo desde la UI

Cuando vayamos a crear un servicios mediante la UI deberemos seleccionar: `+ componer`,  introducir el docker compose y darle a `desplegar`. Una vez desplegado el servicio. Dockge genera la carpeta del servicio en `/srv/docker/stacks`, el compose.yml y los volumenes automáticamente.

<p align="center">
  <img src="../../assets/img/dockge/crear.png" alt="Creación docker" width="800">
</p>

Una vez desplegado el servicio. Dockge genera la carpeta del servicio en `/srv/docker/stacks`, el compose.yml y los volumenes automáticamente.

<p align="center">
  <img src="../../assets/img/dockge/ls.png" alt="Creación docker" width="800">
</p>


#### Importar uno existente: 

Para importar uno existente en caso de ser necesario. 

Deberemos crear una carpeta dentro de `/srv/docker/stacks/<nombre-del-servicio>` y colocar su compose.yml y pegar los volumenes para no perder información. Una vez creada Dockge lo detectará y podrá gestionarlo.


### Flujo de trabajo recomendado

Para nuestro uso el flujo de trabajo más recomendado es:

1. Crear/organizra las carpetas en `/srv/docker/stacks`.

2. Desde Dockge, crear o importar el stack.

3. Editar el compose.yml desde la UI de Dockge (más cómodo y escalable).

4. Desplegar, iniciar y pausar directamente desde el panel.

5. Repetir para cada servicio del ecosistema.

## Conclusión

Dockge es uno de los servicios más esenciales de los que disponemos, ya que podríamos considerarlo como el centro de mando de bitCLD desde donde podemos arrancar, reiniciar y todas las demás funciones que necesitemos. Además, el poder gestionar los compose.yml directamente desde su interfaz siendo muy intuitivo.

