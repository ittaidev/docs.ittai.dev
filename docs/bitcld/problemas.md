# Problemas detectados y soluciones aplicadas 

Debido a la naturaleza de nuestro proyecto han existido diversos problemas de los que vamos comentar qué fueron y cómo lo solucionamos.

## Cloudflare
Con la plataforma Cloudflare tuvimos un problema de comprensión inicial, debido a que:

- Pensábamos que deberíamos tener un túnel para cada servicio, lo cuál es completamente innecesario y desorganizado. 

- Lo solucionamos viendo vídeos del proceso de generación de túneles y viendo la documentación oficial.

- No sabíamos cómo modificar la pantalla de login de Access para darle un toque más profesional ni tampoco añadir el logo una vez que encontramos la opción.

- Para solucionarlo estuvimos investigando dentro de las propias aplicaciones hasta que lo encontramos, pero nos costó bastante ya que no encontramos vídeos y no lo llegamos a ver en la documentación. Para el logo fue más simple cuando empezamos a utilizar GitHub.

## Dockge
- El principal problema que tuvimos fue que cuando creamos los primeros docker compose de nuestros servicios Dockge no era capaz de gestionarlos por lo que se volvía inútil.

- La solución la encontramos en un vídeo en el que descubrimos que no habíamos ajustado bien el docker.sock ni el environment. Una vez ajustados ya podíamos gestionarlos desde Dockge sin problemas.

- Otro problema fue que no podíamos iniciar un servicio ya que una dirección IP ya está siendo utilizada, en el caso del servicio que usa una IP estática, Vaultwarden.

- La solución que encontramos es iniciarlo primero o en su defecto más adelante poner a cada servicio una IP estática y así no se nos pisan.

- El último problema que encontramos fue que estábamos repitiendo el mismo puerto en dos servicios distintos y por tanto nos saltaba error en la terminal.

- Lo solucionamos fijándonos bien en los puertos y teniendo un documento donde empezamos a apuntar los puertos.

## Adguard Home / Caddy 
- El problema que tuvimos con Adguard Home fue que no filtraba anuncios ni rastreadores en las páginas en la que entrábamos

- La solución fue añadir las listas personalizadas de filtrado, a partir de ese momento ya bloqueaba anuncios y rastreadores en páginas como m.youtube.com desde el móvil.

- El otro problema que detectamos fue que cuando poníamos nuestros dominios locales, después de añadir los rewrittes personalizados, nos seguían sin aparecer por lo que no podíamos acceder desde nuestro dispositivo móvil.

- La solución que encontramos fue añadir a Adguard Home en la red de Caddy en el docker compose, una vez agregado ya se podían comunicar y por tanto ya los dominios locales funcionaban sin problemas.

## Nextcloud
- El único inconveniente que tuvimos con Nextcloud fue que desconocíamos que debíamos modificar el archivo config.php  para poder acceder a nuestros enlaces de cld.lan y cld.bitcld.com.

- Una vez añadidos los dominios a trusted_domains  ya no tuvimos ningún problema para poder acceder a ellos.

## Inteligencia Artificial
- El principal problema que tuvimos con la inteligencia artificial en nuestro proyecto fueron nuestras limitaciones de hardware para poder correr de forma óptima y rápida modelos locales, encontrando uno que tarda pero que al menos nos responde. 

- La solución más sencilla y completa que encontramos para no descartar la IA en bitCLD fue el uso de una API gratuita de Groq. De esta manera tenemos acceso a inteligencia artificial rápida y con modelos más completos sin gastar dinero.

## Vaultwarden
- Con este servicio el principal problemas que teníamos fue que al tener Caddy y Vaultwarden en dos dockers diferentes no se podían comunicar y por tanto usarlo ya que no disponíamos de un puerto como tal.

- La solución fue crear nuestra red Caddy_net gracias a la documentación oficial de la web.

- Otro problema con el que nos encontramos fue el monitorizar y saber cómo implementarlo con Cloudflare Tunnel y con Caddy en su CaddyFile.

- Para solucionarlo, le pusimos una IP estática al contendor para poder tenerlo siempre localizado y poner el puerto que está “expuesto”. 

## Uptime Kuma
- A pesar de ser bastante simple, no comprendimos bien como usar las notificaciones de Uptime Kuma ya que nunca habíamos trabajado con Telegram ni bots.
- Gracias a un vídeo en youtube que nos explicó paso a paso como hacerlo pudimos tenerlas configuradas sin problemas.
  
## Netdata
- Con Netdata, al igual que con Uptime Kuma, no fuimos capaces de configurar las notificaciones al comienzo.

- Lo terminamos arreglando cuando creamos la cuenta de Netdata, ya que al principio no la habíamos creado, y buscando en las opciones, una vez creada, la encontramos y pudimos configurarlas con un bot de Telegram.

## GitHub, GitHub Pages y Cloudflare Pages
- Al principio nunca habíamos desplegado una página web en internet y mucho menos usado GitHub ni los distintos Pages por lo que todo fue un lío y no nos aclarábamos bien.

- Gracias a vídeos explicativos y a mirar en las documentaciones respectivas de cada uno pudimos desplegar tanto nuestra página web, que sirve como presentación del proyecto, y nuestra documentación para darle un toque más profesional.
