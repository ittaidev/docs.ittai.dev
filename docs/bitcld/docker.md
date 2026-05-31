# Docker y docker compose
## Introducción

Docker es una plataforma de virtualización ligera que está basada en contenedores lo cuál es una gran ventaja ya que nos permite desplegar nuestros servicios de forma simple, aislada y escalable. Debido a que se basa en imágenes cada servicio se encarga de sus dependencias sin interferir en nuestro sistema base ni en otros contenedores. Docker compose, por otro lado, necesita de docker para funcionar ya que docker se encarga de ejecutar los contenedores y docker compose facilita la configuración y el arranque conjunto de varios contenedores relacionados y también se encarga de los puertos, volúmenes y redes.

## Motivos de elección

Optamos por usar docker y docker compose ya que es bastante estable y muy adoptado con una gran comunidad detrás. Necesitábamos un entorno para desplegar y ejecutar nuestras aplicaciones y estas dos tecnologías satisfacen por completo nuestras necesidades. Nos ofrecen también soporte nativo en Linux, lo cuál era esencial para nosotros. Además, estas tecnologías están perfectamente alineadas con los pilares de nuestro proyecto ya que es algo simple, es seguro y lo podemos auto hospedar.

## Función dentro del proyecto

Dentro de nuestro proyecto bitCLD, actúan como el entorno de ejecución principal de todos los servicios. Ya que cada aplicación se ejecuta en su propio contenedor, como iremos viendo más adelante en los distintos servicios que usamos y también permite mantener un sistema limpio, ordenado y fácil de administrar gracias a Dockge, el cual es uno de los servicios que tenemos en el proyecto y nos permite administrar todo aún más sencillo.


## Instalación de Docker y Docker Compose
En bitCLD hemo optado por una instalación directa desde los repositorios oficiales de Ubuntu, buscando que sea de la manera más simple y estable. Con este método, tanto Docker como Docker Compose se instalan con un solo comando, sin necesidad de añadir repositorios externos.

### 1. Instalación de Docker y Docker Compose

Ejecutamos el siguiente script oficial para instalar tanto Docker como docker compose en su versión V2:

    curl -fsSL https://get.docker.com | sudo sh

<p align="center">
  <img src="../../assets/img/docker/inst.png" alt="docker" width="600">
</p>

>Antes de que ejecutemos este script debemos asegurarnos que no tengamos instalado Docker anteriormente porque podría rompernos configuraciones que ya tengamos. En nuestro caso al ser la primera vez que instalamos no hay problema.

### 2. Habilitar y arrancar el servicio

A continuación, activamos el servicio de Docker para que se inicie automáticamente al arrancar el sistema y lo iniciamos manualmente por primera vez:

        sudo systemctl enable docker
        sudo systemctl start docker


Podemos verificar su estado con:

        systemctl status docker

<p align="center">
  <img src="../../assets/img/docker/status.png" alt="docker version" width="600">
</p>

## Verificación del funcionamiento de Docker

### 1. Ver versión y servicios activos

Compruebamos que tanto Docker Engine como Compose están disponibles:

        docker version
        docker compose version

<p align="center">
  <img src="../../assets/img/docker/version2.png" alt="docker version" width="300">
</p>

<p align="center">
  <img src="../../assets/img/docker/docker-compose-version.png" alt="docker compose version" width="400">
</p>



### 2. Ver lista de contenedores activos y detenidos

        docker ps         # activos
        docker ps -a      # todos

<p align="center">
  <img src="../../assets/img/docker/ps.png" alt="docker ps" width="900">
</p>

<p align="center">
  <img src="../../assets/img/docker/ps-a.png" alt="docker ps -a" width="900">
</p>

### 4. Listar imágenes descargadas

        docker images

<p align="center">
  <img src="../../assets/img/docker/images.png" alt="docker images" width="600">
</p>

---

## Conclusión

En conclusión Docker y docker compose son una base fundamental de bitCLD, ya que sin estas el despliegue, ejecución y mantenimiento de aplicaciones se haría mucho más complejo y tedioso. Gracias al sistema de contenedores mantenemos nuestro sistema limpio, estable y nos aseguramos que no hayan conflictos entre las aplicaciones.

Combinado con Dockge, que nos ofrece una gestión mucho más visual y automatizada, es una gran solución para bitCLD. Con estas tecnologías cumplimos nuestros pilares: simplicidad, seguridad, autogestión y usar herramientas de código abierto siempre que sea posible.
