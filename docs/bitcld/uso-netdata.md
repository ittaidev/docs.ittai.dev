# Netdata

Netdata es muy similar a Uptime kuma, en el sentido que también también tiene una interfaz para monitorear sólo que en este caso es al servidor. Además, no corre en un docker sino en nuestro servidor. Para poder ver el estado de nuestro equipo en tiempo real deberemos:

1. Acceder a `stats.lan`  en local o a [stats.bitcld.com](http://stats.bitcld.com/). 
2. Desde la interfaz tendremos acceso a estado en tiempo real de cualquier componente de nuestro equipo (CPU, RAM, Almacenamiento…)

    <p align="center">
    <img src="../../assets/img/netdata/uso1.png" alt="stats.bitcld.com" width="600">
    </p>

3. Además, al igual que Uptime Kuma, también tiene la función de mandarnos notificaciones de forma pasiva al mismo grupo de Telegram por si existiera algún problema con el servidor.

<p align="center">
  <img src="../../assets/img/netdata/noti5.png" alt="stats.bitcld.com" width="600">
</p>
