# Adguard Home

## Introducción

Adguard Home es un servidor DNS local,  de código abierto que se encarga de bloquear publicidad, rastreadores y sitios potencialmente peligrosos. A diferencia de los bloqueadores convencionales, Adguard Home los bloquea antes de que lleguen al ordenador. 

## Motivos de elección

Elegimos Adguard Home debido a que nos permite personalizar qué deseamos bloquear mediante listas personalizadas y sobretodo por ser de código abierto ya que nos garantiza transparencia. 

## Función dentro del proyecto

En bitCLD, Adguard Home funciona como nuestro servidor DNS interno, se encuentra desplegado como un contenedor y es gestionado por Dockge. En nuestro caso no lo usamos a nivel de red, sino que conectamos nuestros dispositivos a él.

## Instalación

Adguard Home se despliega fácilmente como contenedor Docker y se gestiona desde Dockge, manteniendo la coherencia y modularidad del ecosistema bitCLD.
Desde la interfaz de Dockge, creamos una nueva pila (stack) llamada adguard y añadimos el siguiente contenido en su archivo docker-compose.yml:

        services:
        adguardhome:
            image: adguard/adguardhome:latest
            container_name: adguardhome
            restart: unless-stopped
            ports:
            - 192.168.1.5:53:53/tcp
            - 192.168.1.5:53:53/udp
            - 100.71.42.126:53:53/tcp
            - 100.71.42.126:53:53/udp
            - 192.168.1.5:3000:3000/tcp
            volumes:
            - ./workdir:/opt/adguardhome/work
            - ./confdir:/opt/adguardhome/conf
            environment:
            - TZ=Atlantic/Canary
            networks:
            - caddy_net
        networks:
        caddy_net:
            external: true

### Explicación del docker compose

            image: adguard/adguardhome:latest
            container_name: adguardhome
            restart: unless-stopped

| Parámetro | Descripción | 
|----------|---------|
| `image: adguard/adguardhome:latest` | Usa la última versión oficial de la imagen de Adguard Home disponible desde Docker Hub. | 
| `container_name: adguardhome` | Asigna un nombre fijo al contenedor (útil para comandos y depuración). | 
| `restart: unless-stopped` | Hace que el contenedor se reinicie automáticamente si se detiene de forma inesperada, salvo que se haya detenido manualmente. | 

            ports:
            - 192.168.1.5:53:53/tcp
            - 192.168.1.5:53:53/udp
            - 100.71.42.126:53:53/tcp
            - 100.71.42.126:53:53/udp
            - 192.168.1.5:3000:3000/tcp

| Puerto | Descripción | 
|----------|---------|
| `- 192.168.1.5:53:53/tcp` `- 192.168.1.5:53:53/udp` | DNS local en LAN, publica DNS sobre TCP y UDP en la IP LAN del host. | 
| `- 100.71.42.126:53:53/tcp` `- 100.71.42.126:53:53/udp` | DNS accesible por Tailscale, Publica DNS sobre TCP y UDP, en la IP Tailscale del host  | 
| `- 192.168.1.5:3000:3000/tcp` | Panel web para administración, Expone la UI del asistente en :3000 por la IP LAN. | 

> Los binds 192.168.1.5:53 y 100.71.42.126:53 limitan la escucha a esas interfaces, ideal para separar LAN y Tailscale.

            environment:
            - TZ=Atlantic/Canary
         
| Environment | Descripción | 
|----------|---------|
| `- TZ=Atlantic/Canary` | Zona horaria del contenedor afecta a logs y estadísticas. |


        networks:
        caddy_net:
            external: true

| Red | Descripción | 
|----------|---------|
|`caddy_net` | El servicio se conecta a la red caddy_net, lo que permite que Caddy se comunique con otros contenedores |
|`external: true` | Indica que la red caddy_net ya existe y no será creada por este docker-compose. Esto permite compartir la misma red entre distintos docker-compose.yml, haciendo posible que varios contenedores se comuniquen entre sí. |

## Configuración

### 1. Acceso a la interfaz

En nuestra red local:

        http://192.168.1.5:3000

Creamos un usuario y una contraseña y ya tendremos acceso al panel de administración de Adguard Home.

Una vez configurado Caddy server, podremos acceder a la interfaz desde:

        https://dns.lan

<p align="center">
  <img src="../../assets/img/adguard/panel2.png" alt="Acceso a la interfaz" width="800">
</p>


### 2. Upstreams DNS

Los upstreams son los servidores externos a los que AdGuard consulta cuando no tiene la respuesta en caché. Por privacidad y seguridad se ha seleccionado el DNS de Quad 9:

Ya que nos garantiza que nos ofrece: 

- Seguridad: proporciona bloqueo de amenazas como malware o phisihing, Quad9 verifica el sitio con una lista de dominios combinados de muchos socios de información sobre amenazas diversas. En función de los resultados, completa o rechaza el intento de búsqueda, impidiendo las conexiones a sitios maliciosos.

- Privacidad: al no conservar datos personales del usuario no hace falta ningún registro y no no se almacenan ni transmiten direcciones IP fuera de sus sistemas. Además, Quad9 tiene su sede en Suiza, donde las leyes estrictas regulan estas promesas.

- Rendimiento: los sistemas de Quad9 están distribuidos por todo el mundo en más de 183+ sedes en 90 países. lo que significa que la distancia y el tiempo necesarios para obtener respuestas, son menores que casi cualquier otra solución.

<p align="center">
  <img src="../../assets/img/adguard/upstream.png" alt="Upstreams DNS" width="800">
</p>


> Por privacidad, conviene elegir servidores seguros y sin registro, preferiblemente con DoT (DNS over TLS) o DoH (DNS over HTTPS).

### 3. Listas de bloqueo

Una lista de bloqueo DNS (o blocklist) es un conjunto de dominios que el servidor DNS considera no deseables. Si un dominio aparece en una lista de bloqueo, el servidor devuelve una respuesta vacía o falsa (0.0.0.0), impidiendo que la conexión se realice. Este método evita que los anuncios, rastreadores, mineros o webs maliciosas se carguen siquiera, antes de que lleguen al navegador.
A diferencia de un bloqueador de anuncios como Ublock Origin (que actúa en el navegador después de recibir el contenido), el filtrado DNS:

- Protege toda la red (móviles, televisores, tablets… incluso los que no tienen bloqueador).

- Reduce el tráfico (porque las conexiones no deseadas nunca salen).

- Aumenta la privacidad, ya que evita que se comuniquen con terceros que recolectan datos.

Para establecer listas de bloqueo deberemos irnos a `Filters > DNS blocklist > Add > Add a custom list`

Y añadir las listas que consideremos. En nuestro caso se han seleccionado estas listas:

        https://adguardteam.github.io/HostlistsRegistry/assets/filter_1.txt

        https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt

        https://raw.githubusercontent.com/StevenBlack/hosts/master/hosts

        https://adguardteam.github.io/HostlistsRegistry/assets/filter_50.txt

<p align="center">
  <img src="../../assets/img/adguard/listas.png" alt="Upstreams DNS" width="800">
</p>

> Estas son las listas que hemos seleccionado pero existen una gran cantidad de ellas, lo ideal es ir probando y añadir o quitar las que no nos interesan
> 
> Debemos evitar combinar listas enormes redundantes si no necesitamos agresividad extrema ya que podría romper ciertos sitios.
> 
> Si algo rompe un sitio: Primero deberemos mirar el Query Log para ver el dominio bloqueado, añadir un Allowlist puntual y si es recurrente, desactiva temporalmente la lista sospechosa para confirmarlo.

### 4. Activar DNSSEC

El DNSSEC (Domain Name System Security Extensions) añade una capa de verificación criptográfica a las respuestas DNS. Evita ataques como el DNS spoofing, donde un atacante intenta falsificar una dirección DNS.

Para activarlo deberemos ir a `Settings → DNS Settings → Enable DNSSEC`

<p align="center">
  <img src="../../assets/img/adguard/dnssec.png" alt="dnssec" width="800">
</p>

### 5. DNS Rewrites: para dominios internos .lan

Un DNS rewrite sustituye una dirección de dominio por una IP fija. En nuestro caso, los usamos para que nuestros servicios internos (`dns.lan`, `pass.lan`, `monitor.lan`...) apunten a nuestro servidor local `192.168.1.5`:


<p align="center">
  <img src="../../assets/img/adguard/rewrite.png" alt="dnssec" width="800">
</p>

Esto facilita la integración con [Caddy](./caddy.md), que recibe el tráfico local y lo distribuye al contenedor correspondiente por la red interna Docker (caddy_net).


## Funcionamiento de Adguard Home

Una vez tengamos todo configurado ya podremos usar nuestro DNS, para ello deberemos establecerlo como nuestro DNS principal y a partir de ese momento ya podremos tener todas las ventajas de que nos ofrece Adguard Home.

<p align="center">
  <img src="../../assets/img/adguard/ip.jpeg" alt="dnssec" width="250">
</p>

Una vez establecido como servidor DNS, Adguard Home bloqueará anuncios y rastreadores como por ejemplo los anuncios en los vídeos de YouTube, simpre y cuando se abra en el navegador y no en la aplicación:

<p align="center">
  <img src="../../assets/img/adguard/noads.jpeg" alt="Adguard Panel" width="250">
</p>

Y desde el propio panel podemos observar qué dominios ha permitido y cuales ha bloqueado:

<p align="center">
  <img src="../../assets/img/adguard/bloqueo1.png" alt="Adguard Panel" width="800">
</p>

<p align="center">
  <img src="../../assets/img/adguard/bloqueo2.png" alt="Adguard Panel" width="800">
</p>

<p align="center">
  <img src="../../assets/img/adguard/bloqueo3.png" alt="Adguard Panel" width="800">
</p>

## Conclusión

Adguard Home es un servicio esencial para añadir a nuestro ecosistema ya que nos ofrece privacidad y control sobre nuestras consultas de una manera muy simple. Además al ser autoalojado tenemos la oportunidad de iniciarlo o detenerlo fácilmente desde Dockge. Es por ello que es una gran incorporación a bitCLD ya que respeta los cuatro pilares del proyecto.

