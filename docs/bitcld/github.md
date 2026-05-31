# GitHub

## Introducción

GitHub es una plataforma de alojamiento de código y proyectos basada en Git, un sistema de control de versiones que permite guardar, organizar y seguir la evolución de cualquier archivo, especialmente de desarrollo de software y documentación. A través de repositorios, ramas y commits, GitHub facilita trabajar de forma ordenada, colaborar con otras personas, revisar cambios y mantener un historial completo de todo lo que se hace en un proyecto. Además, incluye funciones adicionales como seguimiento de issues, gestión de versiones y publicación de sitios web estáticos mediante GitHub Pages, lo que lo convierte en una herramienta muy completa para centralizar y proteger el trabajo de desarrollo y documentación.

## Motivo de elección

Hemos elegido GitHub ya que es una solución que se usa mucho en entornos profesionales y me quería tener contacto con ella para poder acostumbrarme a su uso. Además gracias a GitHub Pages nos permite publicar nuestra web sin ningún coste adicional y nos ofrece una alta disponibilidad para tener una copia de seguridad de nuestra web y nuestra documentación web.

## Función dentro del proyecto

Dentro de de bitCLD, GitHub cumple tres funciones principales ya que actúa como repositorio para tener un backup de nuestra documentación dándonos también la oportunidad de conectar Cloudflare Pages con el respositorio y así cualquier cambio que hagamos en GitHub se vea reflejado. En segundo lugar, nos permite almacenar el código de la página web del proyecto. Y, en tercer lugar, GitHub Pages la usamos para publicar la web del proyecto de forma estática, sirviéndonos para darle una imagen más profesional al proyecto, sin necesidad de tener que gastar dinero en hostings.

## Creación de repositorio

Para la creación de un repositorio es muy sencillo, simplemente debemos crearnos una cuenta en GitHub y seleccionar New Repository

Deberemos añadir un nombre, y escoger la visibilidad. En este caso seleccionaremos público ya que es para la página web de nuestro proyecto, sin embargo, cuando la usamos como copia de seguridad los ponemos en privado.

<p align="center">
  <img src="../../assets/img/github/repo1.png" alt="GitHub" width="600">
</p>

> Si vamos a usar GitHub Pages el respositorio deberá tener la visibilidad en Público

Una vez creado para añadir archivos deberemos seleccionar Add file > Upload files

<p align="center">
  <img src="../../assets/img/github/repo2.png" alt="GitHub" width="400">
</p>

Una vez subidos los archivos ya tendremos nuestra copia de seguridad de nuestra página web. También podemos añadir un Readme para que quede más visual para nosotros mismos y para cualquier persona puede tener una idea de qué hay en el repositorio a simple vista.

<p align="center">
  <img src="../../assets/img/github/repo3.png" alt="GitHub" width="600">
</p>

## Publicación de la web

Para publicar nuestra web mediante GitHub, en el repositorio público que deseemos, seleccionamos Settings > Pages

En Build and deployment dejaremos la opción de Deploy from a brach, escogeremos la branch main y dejaremos marcado el Enforce HTTPS.


<p align="center">
  <img src="../../assets/img/github/repo5.png" alt="GitHub" width="600">
</p>


Una vez hecho después de unos momentos ya tendremos nuestro sitio web publicado en internet y podremos acceder a él mediante el enlace que nos ofrece GitHub.

<p align="center">
  <img src="../../assets/img/github/repo6.png" alt="GitHub" width="600">
</p>

En nuestro caso, al disponer del dominio `bitcld.com`. Lo añadiremos en Custom domain, para ello primero deberemos añadir los registros DNS correspondientes en Cloudflare

<p align="center">
  <img src="../../assets/img/github/repo7.png" alt="GitHub" width="600">
</p>

Añadimos el dominio y una vez haya replicado, ya tendremos nuestra página web desplegada a través de nuestro dominio personalizado. En [https://bitcld.com/](https://bitcld.com/).

<p align="center">
  <img src="../../assets/img/github/repo4.png" alt="GitHub" width="600">
</p>


## Conclusión

GitHub lo usamos para poder tener protegido todo el trabajo nuestro proyecto ya que podemos guardar la documentación y nuestra página web. Además, nos permite publicar nuestra web del proyecto mediante GitHub Pages y para conectar a Cloudflare la documentación. Gracias a esto, disponemos de una copia de seguridad en la nube de todos nuestros archivos más importantes, y también tenemos la oportunidad de trabajar con una plataforma profesional que nos permite darnos a conocer como profesionales y dar a conocer el proyecto bitCLD.

