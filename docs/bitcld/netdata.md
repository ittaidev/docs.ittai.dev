# Netdata

## Introducción

Netdata es una plataforma de monitorización a tiempo real y de salud de nuestro servidor, contenedores y aplicaciones. Nos permite poder ver métricas bastante detalladas de la CPU, memoria, red, discos, contenedores y aplicaciones gracias a su interfaz interactiva y que nos lo muestra en tiempo real. Además es una solución ligera.

## Motivos de elección

Optamos por Netdata debido a que necesitábamos una solución que nos permita poder monitorizar nuestro monitor en todo momento y que nos envíe notificaciones si existe alguna incidencia. Netdata también fue escogida ya que, a diferencia de otras es ligera y fácil de configurar. Además se aloja directamente en nuestro servidor por lo que cumple con los principales de bitCLD: simple, seguro, autohospedado.

## Función dentro del proyecto

Dentro de nuestro proyecto, principalmente se encarga de monitorizar nuestro servidor y de enviar notificaciones de alerta críticas. Adicionalmente, también analiza los contenedores pero esa no es su misión principal ya que de eso se encarga Uptime Kuma. Además las notificaciones serán mandadas al grupo de notificaciones donde también se encuentra nuestro bot de Uptime Kuma para centralizar las notificaciones.


## Instalación:
Debido a las características de Netdata la ejecutamos directamente en el servidor para que pueda acceder a todas las métricas de nuestro sistema. La instalación es bastante sencilla.

        sudo apt install curl -y
        bash <(curl -Ss https://my-netdata.io/kickstart.sh)


Este script instala la versión estable de Netdata, configura los servicios necesarios y habilita su inicio automático al arrancar el sistema. Una vez completado, el panel web estará disponible en el puerto 19999 del servidor.


### Acceso al panel
Desde cualquier dispositivo dentro de la red local, podemos acceder a la interfaz web escribiendo en el navegador, en nuestro caso: `http://192.168.1.5:19999`

<p align="center">
  <img src="../../assets/img/netdata/panel.png" alt="Panel Netdata" width="600">
</p>

<p align="center"><em>Panel Netdata</em></p>

## Configuración
Por defecto, Netdata comienza a recopilar métricas del sistema, red y contenedores Docker sin requerir pasos adicionales. En caso de querer personalizar la frecuencia de muestreo, los módulos activos o la retención de datos, los archivos de configuración se encuentran en:

        /etc/netdata/netdata.conf

<p align="center">
  <img src="../../assets/img/netdata/conf.png" alt="/etc/netdata/netdata.conf" width="600">
</p>

<p align="center"><em>/etc/netdata/netdata.conf</em></p>

Para aplicar cualquier cambio ejecutamos:

        sudo systemctl restart netdata


### Creación de cuenta

Para la creación de una cuenta en Netdata deberemos ejecutar el siguente comando en la consola del servidor:

    sudo cat /var/lib/netdata/netdata_random_session_id

Y copiar y pegar el ID de sesión

<p align="center">
  <img src="../../assets/img/netdata/cuenta2.png" alt="Cuenta Netdata" width="500">
</p>
<p align="center">
  <img src="../../assets/img/netdata/cuenta.png" alt="Cuenta Netdata" width="500">
</p>
<p align="center"><em>Cuenta Netdata</em></p>

Seleccionamos con que cuenta queremos vincularlo, el nombre y ya estaría sincronizado

#### Ventajas de usar una cuenta de Netdata

Aunque en bitCLD utilizamos Netdata de forma local y autogestionada, disponer de una cuenta de Netdata Cloud puede aportar beneficios adicionales en ciertos escenarios de monitorización avanzada o entornos distribuidos.

Estas son las principales ventajas:

- Panel centralizado: permite visualizar en una sola interfaz el estado de varios servidores o nodos, sin necesidad de conectarse a cada uno de forma individual.

- Acceso remoto seguro: se accede a las métricas desde la nube de Netdata mediante un canal cifrado, sin exponer puertos ni abrir accesos adicionales en el servidor.

- Alertas unificadas: facilita la gestión de notificaciones y la integración con servicios externos (Slack, Discord, Telegram, etc.) directamente desde la plataforma.

- Historial extendido de métricas: guarda datos durante más tiempo que la instalación local por defecto, lo que permite analizar tendencias y comparativas a largo plazo.

- Análisis avanzado: incluye funciones de diagnóstico y detección de anomalías impulsadas por IA, que ayudan a identificar patrones de comportamiento anómalos en el sistema.

- Gestión multiusuario: permite otorgar acceso a otros administradores o colaboradores con diferentes niveles de permisos.

### Notificaciones

Para la creación de notificaciones deberemos: 

1. Irnos a space settings
2. Alert y notifications
3. +Add configuration
4. Seleccionar que integración usaremos, en nuestro caso Telegram, pero existen muchas más (por email, aplicación de móvil, slack, teams...)
5. Introduciremos un nombre y el API de nuestro bot junto con el Chat ID de nuestro grupo. Además de indicar cada cuánto tiempo las queremos y en que tipo de notifiaciones estamos interesados
  <p align="center">
  <img src="../../assets/img/netdata/noti2.png" alt="stats.bitcld.com" width="450">
  </p>

> Para más información sobre la creación de bots y configuración con grupo de Telegram [acceder con este enlace](./uptime-kuma.md), a la documentación de Uptime Kuma en la sección de Creación de bot.

<p align="center">
  <img src="../../assets/img/netdata/noti3.png" alt="stats.bitcld.com" width="700">
</p>

Una vez hecho ya recibiremos las notificaciones pertinentes en nuestro canal de telegram junto con las del bot de Uptime Kuma.

<p align="center">
  <img src="../../assets/img/netdata/noti4.png" alt="stats.bitcld.com" width="800">
</p>
<p align="center">
  <img src="../../assets/img/netdata/noti5.png" alt="stats.bitcld.com" width="800">
</p>


## Conclusión

Netdata nos aporta una visión a tiempo real de la salud de nuestro sistema, pudiendo detectar anomalías antes de que impida el correcto funcionamiento de nuestro servidor.
Su bajo consumo en recursos, su facilidad de uso y la posibilidad de recibir notificaciones por Telegram lo hacen ideal para nuestro proyecto.
