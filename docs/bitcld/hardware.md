# Hardware y especificaciones

## Introducción

Como servidor para nuestro proyecto hemos elegido el GMKtec NucBox G2. Este equipo es un minipc bastante pequeño con componentes modernos y sobretodo eficientes. Aunque según el fabricante está pensado para un uso de escritorio nosotros lo usaremos como nuestro servidor. Dispone de un procesador Intel N100 de 4 núcleos, memoria de 12 GB DDR5 y 512 GB de almacenamiento SSD.

## Motivos de elección

Elegimos este minipc por delante de otras opciones por tres razones principales: 

La primera y principal razón por la que lo escogimos fue su precio ya que tras una larga investigación para elegir el equipo este era la opción más potente disponible por menos de 100 €. 
El segundo motivo fue por su bajo consumo energético, ya que al tratarse de un servidor, la idea es que lo tengamos operativo a diario o al menos durante un largo tiempo por lo que debía consumir muy poco para evitar gastos en electricidad y en sobrecalentamiento del dispositivo.
El último factor por el que nos decidimos por esta opción fue por su memoria RAM, ya que por esos precios sólo encontrábamos equipos con 8 GB de RAM y teníamos miedo de que no fueran suficientes.

## Función dentro del proyecto
Este equipo actúa como el servidor principal de nuestro proyecto actualmente. Se encarga de desplegar y mantener iniciados todos nuestros servicios organizados en contenedores. Además, también funciona como nuestro enlace seguro al exterior gracias al túnel de Cloudflare quitándonos la necesidad de abrir puertos. Y finalmente, como gestor de inteligencia artificial ya que como no tenemos la potencia suficiente lo usamos como intermediario a través de nuestra API de Groq liberándonos carga de procesamiento.

## Componentes

<p align="center">
  <img src="../../assets/img/hardware/1.png" alt="Hardware y especificaciones" width="500">
</p>

Nuestro servidor consiste en un mini pc GMKtec G2 que tiene las siguientes características:

| Componente | Descripción |
|----------|---------|
| Procesador | Intel de 12ª generación Alder Lake N100 | 
| RAM | 12GB DDR5 | 
| Almacenamiento | 512GB M.2 | 
| Puertos | x3 USB A 3.2, x2 HDMI, x1 Display Port, x2 RJ45 |
| Almacenamiento extra | 1 Tb HDD |

### Procesador Intel de 12ª generación Alder Lake N100

Es la mayor limitación que tenemos en nuestro servidor ya que para la mayoría de los servicios de los que disponemos vamos con bastante margen como lo es Caddy, Tailscale, AdGuard Home, Vaultwarden… Pero en servicios como Nextcloud si que notamos un mayor uso del procesador pero es perfectamente usable. Sin embargo, sin lugar a dudas, donde más limitados estamos es con el tema de la inteligencia artificial cuando la corremos en local. De hecho inicialmente se tenía pensado usar IA sólo en local, pero debido a la imposibilidad de uso, ya que es extremadamente lento y consume demasiados recursos, optamos por añadir IA a través de una API para poder tener algo medianamente funcional.

Además, también se nos hace imposible virtualizar entornos como Proxmox, es por eso que optamos por trabajar directamente en Ubuntu Server con contenedores Docker.

### Memoria RAM 12GB DDR5 
En cuanto a RAM vamos bastante bien para cubrir las necesidades de nuestro proyecto actual. Al ser DDR5 nos permite una mayor fluidez entre el CPU y la memoria, por lo que la experiencia de uso nos es bastante positiva. 

Sin embargo, el mayor inconveniente es que la RAM está soldada, lo que nos reduce enormemente la posibilidad de escalabilidad en el futuro. Aunque teniendo en cuenta el procesador del que disponemos es más probable que en caso de necesitar cambiar a otro equipo fuera por las limitaciones de la CPU y no de la RAM.

### Almacenamiento 512GB M.2
En cuanto al almacenamiento principal, el que sea un M.2 es una gran ventaja para nosotros ya que nos permite tener una muy buena velocidad de arranque y que nos cargue los contenedores también bastante rápido. 

La mayor desventaja después de hacer algunas investigaciones es la disipación térmica, es por ello que Netdata es tan importante para poder controlar la temperatura de nuestro disco y que nos envíe alertas si fuera necesario. 

### Conectividad y puertos
En cuanto a la conectividad dispone de WIFI 6, sin embargo, al usarlo como servidor, por estabilidad estará siempre conectado por cable. En cuanto a los puertos tenemos todo lo que necesitamos ya que disponemos de 3 puertos USB 3.0 para poder añadir hasta 3 discos duros externos. 

Además, también disponemos de 2 puertos RJ45 lo cuál nos permitiría segmentar el tráfico de nuestro equipo, por ejemplo, un puerto para el almacenamiento y otro para los servicios. Sin embargo, en nuestro proyecto sólo usaremos un puerto para todo, pero más adelante podríamos mirarlo. 

Los puertos de HDMI y Display, nos vinieron bien cuando tuvimos que hacer las primeras configuraciones ya que de fábrica nos vino con Windows 11 pro, pero una vez que hicimos la instalación de Ubuntu ya no los hemos vuelto a utilizar. 

### Disco duro externo 1 TB HDD
Optamos por añadir un disco duro externo, para aumentar nuestra capacidad de almacenamiento principalmente para nuestro servicio de Nextcloud. El que dispongamos de 3 puertos USB 3.0 es una gran ventaja ya que podemos añadir más almacenamiento. 

La principal desventaja es que es un disco duro por lo que la velocidad de lectura y escritura es lenta, y también no tenemos tanta fiabilidad comparado con un disco de estado sólido. Además, es un disco duro con algunos años, por lo que para nuestro proyecto es suficiente para demostrar de que puede llegar a ser capaz bitCLD y para aprender, pero es necesaria una mejora en el almacenamiento, para incluso añadir más adelante un sistema de Raids para asegurar nuestra información.

## Conclusión
Hemos podido comprobar que elegir GMKtec NucBox G2 fue una buena decisión ya que ha cumplido con creces perfectamente para nuestro proyecto actual. 

Gracias a sus componentes hemos tenido un buen equilibrio entre rendimiento y eficiencia energética, pudiendo correr sin problemas todos nuestros servicios, exceptuando la IA local. Además, el añadir un disco duro externo para el almacenamiento fue una buena decisión y también cumple muy bien como intermediario entre nuestros servicios y el exterior mediante cloudflare.
