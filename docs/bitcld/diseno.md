# Diseño del ecosistema

Como nos muestra este diagrama podemos apreciar el funcionamiento de bitCLD. Tendríamos los servicios, que ocupan la columna central y según cómo accedamos a cada servicio el flujo de trabajo cambia.

Si usamos nuestros servicios en nuestra red local accederemos mediante nuestros dominios `.lan`, haciendo uso del reverse proxy (caddy), que nos genera los certificados y contiene las rutas a los dominios internos en el CaddyFile y nuestro servidor DNS (Adguard Home) que se encargará de indicar a cualquier dispositivo que lo como DNS redirigirá las rutas de nuestros dominios `.lan` a cada servicio gracias a los rewrites. 

Sin embargo, si accedemos mediante nuestro dominio personalizado `subdominio.bitcld.com` se usará el túnel que hemos creado para exponerlo a internet sin abrir puertos y Access se encargará de autenticar a los usuarios. 

Este diseño nos garantiza seguridad en todo momento, ya sea en local como en internet ya que accederemos mediante `https://` en ambos casos cifrando nuestra conexión.

<p align="center">
  <img src="../../assets/img/general/diagrama.png" alt="Diagrama de la infraestructura bitcld" width="800">
</p>

<p align="center"><em>Figura 1: Diseño del ecositema </em></p>