#Zoho Mail

Como no es un servicio que esté autoalojado en nuestro servidor, no necesitaremos iniciarlo ya que siempre estará disponible (siempre y cuando no esté caída la red de Zoho). En cuanto a su uso es como la de cualquier otro correo electrónico.

Ahora vamos a realizar pruebas básicas de funcionamiento para comprobar la correcta entrega y recepción de correos bajo el dominio `contacto@bitcld.com`.

## Envío de correo de prueba
Se envió un mensaje desde una de las cuentas configuradas (contacto@bitcld.com) hacia una cuenta externa, en mi caso la cuenta protonmail del proyecto pero podría ser cualquier otro (Gmail, Outlook...).


<p align="center">
  <img src="../../assets/img/zoho/prueba-envio1.png" alt="Cuenta Zoho" width="700">
</p>

<p align="center">
  <img src="../../assets/img/zoho/prueba-envio2.png" alt="Cuenta Zoho" width="700">
</p>

## Recepción de correo  
Realizamos una prueba inversa, enviando un correo desde `bitcld@proton.me` hacia la dirección `contact@bitcld.com`, comprobando que:

- El mensaje llega al buzón en Zoho Mail sin retrasos ni errores.  
- La interfaz web de Zoho muestra correctamente los mensajes y notificaciones.  

<p align="center">
  <img src="../../assets/img/zoho/prueba-recepcion1.png" alt="Cuenta Zoho" width="700">
</p>

<p align="center">
  <img src="../../assets/img/zoho/prueba-recepcion2.png" alt="Cuenta Zoho" width="700">
</p>

### Acceso vía aplicación móvil  
Zoho Mail ofrece acceso tanto desde su aplicación web ([mail.zoho.eu](https://mail.zoho.eu)) como desde sus apps móviles oficiales.  
Verificamos la sincronización en tiempo real desde la aplicación.

<p align="center">
  <span>
    <img src="../../assets/img/zoho/movil1.jpg" alt="Cuenta Zoho móvil" width="300" style="margin-right: 10px;">
  </span>
  <span>
    <img src="../../assets/img/zoho/movil2.jpg" alt="Bandeja de entrada móvil" width="300">
  </span>
</p>

Y podemos comprobar que recibimos los correos sin ningún problema desde la aplicación móvil también.
