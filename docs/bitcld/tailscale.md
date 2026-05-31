# Tailscale VPN

## Introducción

Tailscale es una red privada virtual (VPN) que usa el protocolo WireGuard. Está diseñada para crear redes privadas seguras de una manera muy simple entre nuestros dispositivos y nuestro servidor siempre que usen la misma cuenta de Tailscale.

## Motivos de elección

Nos decantamos por Tailscale ya que necesitábamos una solución de VPN y esta es la que mejor cumplía nuestras necesidades ya que es simple, segura y muy accesible. Además de ser una de las opciones más ligeras, lo cuál es esencial en nuestro caso. Otro de los principales motivos que nos llevaron a decantarnos por Tailscale es que ofrece un plan gratuito muy completo y generoso, algo vital a la hora de reducir costos.

## Función dentro del proyecto

En nuestro proyecto, Tailscale funciona como una puerta de acceso secundaria y un canal seguro de administración. Decidimos por compatibilidad y simplicidad que se ejecutara directamente en nuestro servidor y no dentro de un contenedor Docker, dándonos la oportunidad de acceder por SSH en caso de ser necesario desde cualquier ubicación, de forma cifrada y autenticada. 

El acceso a nuestros servicios lo hacemos de forma local mediante nuestros dominios `.lan` y de manera externa mediante los subdominios de `bitcld.com`. Por lo que Tailscale es más una alternativa de acceso seguro adicional ante cualquier imprevisto y para acceder a la consola de comandos de forma remota.

## Instalación

La instalación de Tailscale en nuestro servidor es muy sencilla y no requiere contenedores Docker, ya que se ejecuta directamente sobre el sistema operativo anfitrión (Ubuntu Server en nuestro caso).
A continuación, describimos los pasos realizados para su despliegue y configuración básica:

### 1 Creamos una cuenta en Tailscale

Debemos crear una cuenta en la página web oficial 

[https://tailscale.com/](https://tailscale.com)

Y creamos una cuenta cuenta, nos deja registrarnos con una cuenta de google, microsoft, github o apple. En nuestro caso, por comodidad, usaremos la cuenta de google del proyecto.

### 2 Instalación del paquete Tailscale

Primero, añadimos el repositorio oficial de Tailscale e instalamos el paquete con las siguientes órdenes:

        curl -fsSL https://tailscale.com/install.sh | sh

<p align="center">
  <img src="../../assets/img/tailscale/tail1.png" alt="Tailscale" width="750">
</p>

<p align="center">
  <img src="../../assets/img/tailscale/tail2.png" alt="Tailscale" width="550">
</p>

Este script detecta automáticamente la distribución y configura los repositorios necesarios, instalando la versión más reciente del cliente Tailscale.

## Configuración
### 1. Inicio y activación del servicio

Una vez instalado, habilitamos e iniciamos el servicio para que se ejecute automáticamente en cada arranque en caso de que apaguemos o reiniciemos el servidor:

        sudo systemctl enable tailscaled
        sudo systemctl start tailscaled

Podemos comprobar su estado con:

        sudo systemctl status tailscaled


<p align="center">
  <img src="../../assets/img/tailscale/status.png" alt="Estado de Tailscale" width="750">
</p>


### 2 Autenticación del dispositivo

El siguiente paso consiste en autenticar el servidor dentro de nuestra red Tailscale.
Ejecutamos el siguiente comando y seguimos las instrucciones que aparecen en pantalla:

        sudo tailscale up

<p align="center">
  <img src="../../assets/img/tailscale/tail3.png" alt="Tailscale" width="550">
</p>

El sistema mostrará una URL de autenticación, que abriremos desde cualquier navegador donde hayamos iniciado sesión con nuestra cuenta de Tailscale
Tras aprobar el dispositivo, quedará automáticamente vinculado a nuestra red privada.

<p align="center">
  <img src="../../assets/img/tailscale/dispositivos.png" alt="Tailscale móvil" width="600
  ">
</p>

### 3 Descarga de la aplicación móvil

Para poder conectarnos desde nuestro dispositivo móvil descargaremos la [aplicación de tailscale oficial desde la tienda de aplicaciones ](https://play.google.com/store/apps/details?id=com.tailscale.ipn&hl=es). E iniciamos seción con nuestra cuenta ya creada anteriormente.


<p align="center">
  <img src="../../assets/img/tailscale/movil1.jpeg" alt="Tailscale móvil" width="250">
</p>

### 4 Verificación de conexión

Una vez autenticado, verificamos el estado de la conexión con:

        tailscale status

Esto nos mostrará todos los dispositivos conectados a la red, sus direcciones IP privadas de Tailscale y el estado de la conexión.
A partir de ese momento, podremos conectarnos al servidor mediante su IP Tailscale o nombre de host interno desde cualquier otro equipo autorizado.

<p align="center">
  <img src="../../assets/img/tailscale/status1.png" alt="Tailscale móvil" width="600">
</p>

### 5 Acceso remoto por SSH

Una vez finalizada la autenticación de los dispositivos ya podremos hacer una conexión a nuestro servidor de forma remota desde cualquier dispositivo conectado a la red privada de Tailscale, podemos acceder al servidor mediante SSH de forma segura:

        ssh usuario@nombre-equipo-tailscale

<p align="center">
  <img src="../../assets/img/tailscale/movil2.jpeg" alt="Tailscale móvil" width="250">
</p>

Esto nos permite realizar mantenimiento, reinicios o ajustes de emergencia de manera totalmente cifrada y sin depender de puertos abiertos o túneles externos.

Para poder acceder a la terminal desde el móvil existen diversas aplicaciones, en nuestro caso hemos optado por la [aplicación de terminux ](https://play.google.com/store/apps/details?id=com.termux&hl=es-es), por tratarse de una de las más descargadas y su simpleza.

## Conclusión

Tailscale VPN nos ofrece un acceso remoto seguro y cifrado al servidor, sin necesidad de abrir puertos ni exponer servicios a Internet. Como la ejecutamos directamente en nuestro servidor nos garantiza una conexión estable y no depender de docker y docker compose.

Nuestra VPN os permite realizar mantenimiento, reinicios o ajustes de emergencia de manera totalmente cifrada y sin depender de puertos abiertos o túneles externos lo cuál es excelente y justo lo que necesitábamos. Y se alinéa con nuestros pilares: simple, seguro, autogestionado y de código abierto.
