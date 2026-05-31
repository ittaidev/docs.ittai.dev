# Windows

## Introducción

Para ampliar las capacidades de BitCLD, hemos incorporado Dockur Windows, que permite ejecutar Windows dentro de un contenedor Docker. Esta solución evita la necesidad de máquinas físicas o virtuales completas y se integra de manera natural en la arquitectura basada en Docker del proyecto, compartiendo la misma lógica de despliegue, gestión de redes y volúmenes, y control de acceso.

## Motivos de elección

El principal motivo de elección fue la simplicidad y coherencia con la filosofía de BitCLD: ofrecer un entorno seguro, autohospedado y de código abierto. Dockur Windows nos permitió desplegar un entorno Windows funcional sin complicaciones, a diferencia de soluciones como VirtualBox, Proxmox o VMware, que requieren una infraestructura más compleja. Además, nos da la oportunidad de elegir entre muchas versiones distintas de Windows y Windows Server, lo que aporta flexibilidad según la necesidad de la aplicación que queramos ejecutar. Esta capacidad de selección fue un factor decisivo, ya que permite adaptar el entorno exactamente al software que necesitamos sin añadir nuevas tecnologías al stack existente.

## Función dentro del proyecto

Dentro de BitCLD, este servicio ofrece compatibilidad con software exclusivo de Windows de forma aislada y controlada, manteniendo la coherencia con el resto de servicios. Al estar encapsulado en Docker, es fácil de gestionar, mantener y recrear, permitiendo ampliar las capacidades del proyecto sin comprometer la simplicidad, seguridad y el carácter autoalojado que lo definen.

## Instalación

Dockur Windows se despliega fácilmente como contenedor Docker y se gestiona desde Dockge, manteniendo la coherencia y modularidad del ecosistema bitCLD.

        services:
        windows:
            image: dockurr/windows
            container_name: windows
            environment:
            VERSION: 10l
            REGION: es-ES
            KEYBOARD: es-ES
            devices:
            - /dev/kvm
            - /dev/net/tun
            cap_add:
            - NET_ADMIN
            ports:
            - 8006:8006
            - 3389:3389/tcp
            - 3389:3389/udp
            volumes:
            - ./windows:/storage
            restart: always
            stop_grace_period: 2m


### Explicación del docker compose

            image: dockurr/windows
            container_name: windows
            restart: unless-stopped
            stop_grace_period: 2m

| Parámetro | Descripción | 
|----------|---------|
| `image: dockurr/windows` | Usa la última versión oficial de la imagen de dockurr windows disponible desde Docker Hub. | 
| `container_name: windows` | Asigna un nombre fijo al contenedor (útil para comandos y depuración). | 
| `restart: unless-stopped` | Asigna el tiempo para apagarse correctamente antes de forzar cierre.| 

            environment:
            VERSION: 10l
            REGION: es-ES
            KEYBOARD: es-ES

| Environment | Descripción | 
|----------|---------|
| `VERSION: 10l` | Versión de Windows que queremos, varía en función de la que se desea usar | 
| `REGION: es-ES` | Configuración de la región, en nuestro caso España | 
| `KEYBOARD: es-ES` | Configuración del teclado, en nuestro caso el español de España|

Se pueden instalar una gran variedad de versiones de Windows, en la siguiente tabla se indica el valor y tamaño de cada una:

  | **Valor** | **Versión**            | **Tamaño** |
  |---|---|---|
  | `11`   | Windows 11 Pro            | 7.2 GB   |
  | `11l`  | Windows 11 LTSC           | 4.7 GB   |
  | `11e`  | Windows 11 Enterprise     | 6.6 GB   |
  ||||
  | `10`   | Windows 10 Pro            | 5.7 GB   |
  | `10l`  | Windows 10 LTSC           | 4.6 GB   |
  | `10e`  | Windows 10 Enterprise     | 5.2 GB   |
  ||||
  | `8e`   | Windows 8.1 Enterprise    | 3.7 GB   |
  | `7u`   | Windows 7 Ultimate        | 3.1 GB   |
  | `vu`   | Windows Vista Ultimate    | 3.0 GB   |
  | `xp`   | Windows XP Professional   | 0.6 GB   |
  | `2k`   | Windows 2000 Professional | 0.4 GB   | 
  ||||  
  | `2025` | Windows Server 2025       | 6.7 GB   |
  | `2022` | Windows Server 2022       | 6.0 GB   |
  | `2019` | Windows Server 2019       | 5.3 GB   |
  | `2016` | Windows Server 2016       | 6.5 GB   |
  | `2012` | Windows Server 2012       | 4.3 GB   |
  | `2008` | Windows Server 2008       | 3.0 GB   |
  | `2003` | Windows Server 2003       | 0.6 GB   |

> En nuestro caso debido nuestras limitaciones de hardware nos hemos decantado por Windows 10 LTSC

            devices:
            - /dev/kvm
            - /dev/net/tun


| Devices | Descripción | 
|----------|---------|
| `/dev/kvm` | Permite la virtualización por hardware | 
| `/dev/net/tun` | Permite crear redes virtuales dentro del contenedor (VPN, tunneling) | 

            cap_add:
            - NET_ADMIN

| Cap_add | Descripción | 
|----------|---------|
| `NET_ADMIN` | Necesario para gestión de red avanzada dentro del contenedor | 

            ports:
            - 8006:8006
            - 3389:3389/tcp
            - 3389:3389/udp

| Puerto | Descripción | 
|----------|---------|
| `8006:8006` | Permite la gestión web (Web GUI) | 
| `3389/tcp y udp` | Permite RDP (conexión remota a Windows) | 


Una vez guardado, basta con darle al botón de desplegar y Dockge se encargará de todo para que el servicio se inicie.

## Configuración

Para poder observar el proceso de instalación y configuración deberemos acceder a `192.168.1.5:8006` o al subdominio `win.bitcld.com`.

La configuración es extremedamente simple ya que una vez se despliegue empezará automáticamente el proceso de instalación. 

<p align="center"><img src="../../assets/img/windows/1.png" alt="Windows" width="700"></p> 
<p align="center"><img src="../../assets/img/windows/3.png" alt="Windows" width="700"></p> 
<p align="center"><img src="../../assets/img/windows/4.png" alt="Windows" width="700"></p> 

> Por defecto se creará un usuario llamado `Docker` con la contraseña `admin` si deseamos cambiarlo deberemos añadir lo siguiente a nuestro docker compose:
 
    environment:
        USERNAME: "BitCLD"
        PASSWORD: "Contraseña"

<p align="center"><img src="../../assets/img/windows/5.png" alt="Windows" width="700"></p> 

Una vez terminado el proceso tendremos un entorno Windows completamente funcional para poder trabajar sin restricciones o limitaciones:

<p align="center"><img src="../../assets/img/windows/6.png" alt="Windows" width="700"></p> 


> Durante todo el proceso no ha sido realizar ninguna acción.

## Conclusión

La integración de Windows mediante Dockur en Docker refuerza la filosofía de BitCLD de mantener un entorno simple, seguro y autohospedado. Gracias a esta solución, hemos conseguido disponer de un Windows 10 totalmente funcional sin complicaciones de máquinas físicas o virtuales completas, con acceso remoto, almacenamiento persistente y soporte de virtualización.

Además, nos ofrece flexibilidad y control, permitiéndonos elegir entre distintas versiones de Windows y Windows Server según la necesidad, y gestionarlo fácilmente desde el mismo entorno Docker del proyecto. En pocas palabras, esta incorporación demuestra que es posible ampliar las capacidades del proyecto de manera práctica y eficiente, manteniendo la coherencia técnica y la simplicidad operativa que definen BitCLD.