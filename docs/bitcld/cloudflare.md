# Cloudflare

## Introducción

Cloudflare es una plataforma que ofrece seguridad y rendimiento para aplicaciones web. Un gran número de páginas webs y empresas confían en sus servicios y se diferencian por su gran estabilidad por norma general, salvo durante partidos de la liga donde ahí sí que se ve afectada por los bloqueos de IP masivos de La Liga. 

## Motivos de elección

Hemos escogido Cloudflare debido a cuatro motivos principales, los cuáles son esenciales en nuestro proyecto. 

- Seguridad: La solución de cloudflared tunnel elimina la necesidad de que tengamos que abrir puertos en nuestro servidor, lo cual reduce enormemente las posibilidades de ataque. Además, también nos ofrece sistemas de autenticación mediante Cloudflare Access, que permite restringir el acceso a las aplicaciones mediante identidades verificadas, añadiendo una capa Zero Trust sin necesidad de configurar VPNs o firewalls complejos.

- Simplicidad: Cloudflared Tunnel nos facilita la vida con la exposición de servicios con un solo comando y su posterior gestión desde la página web. Además, los certificados HTTPS y la gestión DNS los automatiza como veremos más adelante, reduciendo casi al mínimo la configuración manual y posibles errores.

- Rendimiento: Debido a que usamos su red CDN global, las solicitudes se enrutan de manera más eficiente y se mejora la velocidad y estabilidad del acceso, incluso desde ubicaciones remotas.

- Precio: Aparte de todo eso también nos proporciona una experiencia profesional sin ningún coste adicional en su plan gratuito el cual es bastante generoso.


## Función dentro del proyecto

Cloudflare es vital para la conectividad externa del proyecto. Ya que por una parte es la plataforma que gestiona nuestro dominio. 

A través de Cloudflared Tunnel, cada servicio que desplegamos en Docker se conecta directamente a la red de Cloudflare, evitando el uso de redirecciones o puertos abiertos y nos permite acceder mediante subdominios personalizados.

Con Cloudflare Access controlamos quién puede entrar a cada aplicación mediante un código OTP que se recibirá en el correo.

Y adicionalmente, Con Cloudflare Pages, Nos da la oportunidad de desplegar páginas web, en nuestro caso la de nuestra documentación. Sin embargo, para no mezclarlo con el proyecto en sí, lo hemos añadido en el Anexo correspondiente de Cloudflare Pages.

## Gestor de dominos

Para una gestión mucho más simple y eficaz optamos por Cloudflare para adquirir nuestro dominio y así facilitar mejor la integración con todo lo que deberemos configurar para que funcione nuestro ecosistema. 

Gracias a ello disponemos de varias funciones en Cloudflare como es la revisión de estadísticas 

<p align="center">
  <img src="../../assets/img/cloudflare/dns1.png" alt="Dominio" width="400">
</p>

## Cloudflare Tunnel

Cloudflared permite conectar el servidor local con la red segura de Cloudflare sin necesidad de abrir puertos. Actúa como un “túnel inverso".

Gracias a eso, los servicios del servidor son accecibles mediante nuestro dominio al público (`pass.bitcld.com`, `cld.bitcld.com`...) evitándonos tener que exponer puertos.

> Para poder vincularlo a un dominio propio, deberemos adquirir un dominio mediante Cloudflare domain o transferirlo desde tengamos el dominio.

### Creación de túnel

Para la creación del tunel usaremos el panel de Cloudflare ya que es más intuitivo. Una vez tengamos nuestra cuenta creada y abierta nos iremos a la opción de Zero Trust.

Una vez en la sección de Zero Trust deberemos ir a la sección de Networks > Tunnels > + Create a tunnel

<p align="center">
  <img src="../../assets/img/cloudflare/creacion-tunel1.png" alt="Certificado en carpeta personal" width="800">
</p>

Una vez dentro seleccionaremos Cloudflared:

<p align="center">
  <img src="../../assets/img/cloudflare/creacion-tunel2.png" alt="Certificado en carpeta personal" width="800">
</p>

Asignaremos un nombre al túnel y seleccionamos el sistema operativo donde lo vamos a instalar, en nuestro caso Debian, y Cloudflare generá un archivo y nos mostrará los pasos que deberemos seguir:

<p align="center">
  <img src="../../assets/img/cloudflare/creacion-tunel3.png" alt="Certificado en carpeta personal" width="800">
</p>

Los primeros comandos son para instalar cloduflared en el dispositivo y el comando de 

        sudo cloudflared service install eyJhIjoiNTE3MmE5OTkwN...

Es para instalar y registrar el túnel en nuestro servidor. Así toda la gestión la podremos hacer desde panel y no manualmente.

Para verificar que está en funcionamiento podemos comprobarlo con el comando:

        sudo systemctl status cloudflared

<p align="center">
  <img src="../../assets/img/cloudflare/creacion-tunel5.png" alt="Certificado en carpeta personal" width="800">
</p>

O en el panel:

<p align="center">
  <img src="../../assets/img/cloudflare/creacion-tunel4.png" alt="Certificado en carpeta personal" width="800">
</p>

> Si el túnel está activo, el panel de Cloudflare nos mostrará el estado Healthy, indicando que la conexión entre el servidor y Cloudflare se ha establecido correctamente.

### Publicación de servicios internos

Una vez que tengamos nuestro túnel ya podremos añadir aplicaciones, para ello en la configuración del túnel iremos a la opción de Published application Routes y seleccionaremos `+ Add a published application route`

<p align="center">
  <img src="../../assets/img/cloudflare/servicios1.png" alt="Certificado en carpeta personal" width="800">
</p>

Indicaremos el subdominio que queramos para la aplicación, el dominio, el tipo de servicio y la dirección IP y el puerto:

<p align="center">
  <img src="../../assets/img/cloudflare/servicios2.png" alt="Certificado en carpeta personal" width="800">
</p>

Una vez creado Cloudflare se encargará de:

- Crear el registro DNS correspondiente (tipo CNAME).

- Asignar el certificado SSL/TLS.

- Rutar el tráfico por el túnel de forma cifrada.

Una vez hecho con cada servicios que queramos exponer ya los tendríamos públicos:

<p align="center">
  <img src="../../assets/img/cloudflare/servicios3.png" alt="Certificado en carpeta personal" width="800">
</p>

> De esta manera el acceso a cada servicio se realiza directamente a través de su dominio público, sin necesidad de abrir ningún puerto en el servidor o en el router.

Cómo podemos observar en los registros DNS de nuestro dominio. Cloudflare ha creado los correspondientes registros CNAME para cada servicio

<p align="center">
  <img src="../../assets/img/cloudflare/aplicacion5.png" alt="Certificado en carpeta personal" width="800">
</p>


## Cloudflare Access

Ahora nuestros servicios están públicos pero cualquiera con el subdominio podría acceder es por ello que necesitamos de Cloudflare Access para poder autentificar usuarios. En nuestro caso se hará mediante un código OTP y con limitación geográfica.

### Crear políticas

Para poder proteger nuestros servicios con Access necesitarmos primeramente crear una política:

Para ello en la sección de Zero Trust seleccionamos la opción de policies y +Add a policy

#### Política de autorización de usuarios

Para la política de autorización de usuarios asignameros un nombre en Action seleccionaremos la opción Allow e indicamos un tiempo de duración de la sesión, en nuestro caso seleccionarmos la opción Same as application session timeout, para que dure lo mismo que la aplicación.

<p align="center">
  <img src="../../assets/img/cloudflare/politica1.png" alt="Certificado en carpeta personal" width="800">
</p>

En Add rules: 

En la opción Selector indicamos Emails y añadimos los correos que deseemos y por seguridad añadimos también la opción de país seleccionando en Country y el país que deseamos, en nuestro caso España.

<p align="center">
  <img src="../../assets/img/cloudflare/politica2.png" alt="Certificado en carpeta personal" width="800">
</p>

> De esta manera ya sólo se podrá a las aplicaciones de España y cada vez que queramos entrar deberemos poner el código OTP que nos habrá llegado al correo.

Una vez creada y con las aplicaciones asignadas nuestra política debería aparecernos así

<p align="center">
  <img src="../../assets/img/cloudflare/politica3.png" alt="Certificado en carpeta personal" width="800">
</p>


### Crear aplicaciones

Una vez creada la política creamos una aplicación para cada servicio ya tenemos expuesto:

Vamos a la sección de Zero Trust en nuestro panel de administración de cloudflare y en Access seleccionamos la opción de Applications > +Add an application y seleccionamos la opción de Self-hosted

<p align="center">
  <img src="../../assets/img/cloudflare/aplicacion1.png" alt="Certificado en carpeta personal" width="800">
</p>

Asignamos un nombre a la aplicación y seleccionamos +Add public hostname y ponemos el subdominio y dominio correspondiente al servicio.

<p align="center">
  <img src="../../assets/img/cloudflare/aplicacion2.png" alt="Certificado en carpeta personal" width="800">
</p>

En Access policies seleccionamos la política que hemos creado anteriormente

<p align="center">
  <img src="../../assets/img/cloudflare/aplicacion3.png" alt="Certificado en carpeta personal" width="800">
</p>

> Es muy importante añadir la política ya que si no se pone, no se podrá acceder al servicio ya que no podremos recibir el código al email.

Seleccionamos Next y Next y ya tendríamos nuestro servicio protegido por Cloudflare Access.

Una vez creadas todas nuestras aplicaciones deberemos asegurarnos de que en todas está con la política asiganda

<p align="center">
  <img src="../../assets/img/cloudflare/aplicacion4.png" alt="Certificado en carpeta personal" width="800">
</p>


### Personalizar la página de Access

Para personalizar la página de Access y darle un toque más profesional es muy sencillo. Seleccionamos cualquier aplicación que hemos creado y en Configure > Experience settings accedemos al enlace del apartado Custom Pages

<p align="center">
  <img src="../../assets/img/cloudflare/custom1.png" alt="Custom página" width="800">
</p>

Una vez dentro seleccionamos Mange en Access login page

<p align="center">
  <img src="../../assets/img/cloudflare/custom2.png" alt="Custom página" width="800">
</p>

En este apartado podemos poner un nombre a nuestra organización, ponerle un logo, añadir algún mensaje y un color para que se vea mucho más profesional

<p align="center">
  <img src="../../assets/img/cloudflare/custom3.png" alt="Custom página" width="800">
</p>

#### Poner logo

Para poder poner el logo necesitamos que la imagen esté subida a internet, la opción más sencilla y sin coste es subirla a Github. Para ello la imagen deseada la subimos a un repositorio público y copiamos el enlace de la imagen:

`https://github.com/ittaidev/bitcld-homepage/blob/main/images/logorecortenb.png`

Y una vez copiado lo modificamos cambiando `github.com` por `raw.githubusercontent.com` y quitamos el /blob

`https://raw.githubusercontent.com/ittaidev/bitcld-homepage/main/images/logorecortenb.png`

De esta manera ya tendríamos el logo para poder ser usado por Cloudflare Access.


### Conclusión

Cloudflare nos permite desplegar un túnel, conectar con el servidor y publicar servicios en cuestión de minutos, sin necesidad de tener que estar poniendo comandos complejos. Además, la unión con Cloudflare Access refuerza la seguridad de nuestro ecosistema y también nos permite publicar páginas web mediante Pages. Es por ello que nos decantamos por usar esta tecnología.