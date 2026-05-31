# Servidor Linux

## Introducción

Ubuntu es una distribución de GNU/Linux basada en Debian y es una de las distribuciones más estables, seguras y fáciles de administrar de linux y la versión server es la opción optimizada para entornos de servidor. Además, es de código abierto lo que proporciona un entorno fiable tanto a nivel doméstico como profesional.

## Motivos de elección

Optamos por Ubuntu Server debido a que en su versión sin interfaz gráfica es muy ligera, lo que nos ayuda bastante debido a nuestros limitados recursos. Esta distro es una de las más usadas a nivel empresarial lo que nos permite disponer con una gran documentación y compatibilidad asegurada con nuestras herramientas y servicios. Además elegimos su versión LTS debido a que nos garantiza actualizaciones a largo plazo, ideal para el día a día.

## Función dentro del proyecto

En bitCLD, Ubuntu Server es nuestro sistema operativo principal, por lo que se trata de la base en la que corren todos nuestros servicios. El papel de Ubuntu Server en nuestro proyecto, al igual que cualquier sistema operativo, es ofrecernos un entorno estable, seguro y eficiente para que podamos despreocuparnos del SO y enfocarnos en los servicios, contenedores y conexiones. 


## Instalación 

### 1. Descarga de la imagen ISO
Descargamos la versión Ubuntu Server LTS (por ejemplo, 24.04 LTS) desde la página oficial de Ubuntu:

[https://ubuntu.com/download/server](https://ubuntu.com/download/server)

### 2. Creación del medio de instalación
Quemamos la imagen en una memoria USB mediante herramientas como Rufus, Ventoy o BalenaEtcher, en nuestro caso usamos Ventoy.

<p align="center">
  <img src="../../assets/img/ubuntu/inst.png" alt="Ubuntu" width="500">
</p>

### 3. Instalación en el equipo

Iniciamos desde el USB e instalamos Ubuntu Server siguiendo los pasos del asistente:

- Seleccionamos idioma y disposición del teclado
<p align="center">
  <img src="../../assets/img/ubuntu/inst1.png" alt="Ubuntu" width="700">
</p>

- Elegimos el tipo de instalación

<p align="center">
  <img src="../../assets/img/ubuntu/inst2.png" alt="Ubuntu" width="700">
</p>

- Al estar por cable se conectará a nuestra red y nos saltamos el añadir un proxy

<p align="center">
  <img src="../../assets/img/ubuntu/inst3.png" alt="Ubuntu" width="700">
</p>

- Seleccionamos de donde queremos descargar los paquetes
<p align="center">
  <img src="../../assets/img/ubuntu/inst4.png" alt="Ubuntu" width="700">
</p>
- Creamos nuestro usuario root y le damos nombre al servidor
<p align="center">
  <img src="../../assets/img/ubuntu/inst5.png" alt="Ubuntu" width="700">
</p>
- Activamos la instalación de OpenSSH Server para acceso remoto.
<p align="center">
  <img src="../../assets/img/ubuntu/inst6.png" alt="Ubuntu" width="700">
</p>
- Dejamos las demás opciones por defecto y finalizamos la instalación.

#### 4. Primer inicio y actualización del sistema
Una vez completada la instalación, accedemos al servidor (localmente o por SSH) y actualizamos el sistema:

        sudo apt update && sudo apt upgrade -y
        sudo reboot

### Configuración básica del entorno

#### 1. Red local

Para establecer una IP estática, algo esencial para que todo pueda funcionar sin complicaciones, editamos el archivo de configuración de red y establecemos la ip fija en nuestro servidor:

        sudo nano /etc/netplan/00-installer-config.yaml
        
Y establemos la ip fija en nuestro servidor:

<p align="center">
  <img src="../../assets/img/ubuntu/ip.png" alt="Ip estática" width="500">
</p>

|  | Descripción | 
|----------|---------|
| `renderer: networkd` | Usamos Netplan con el renderer networkd, ideal para servidores sin entorno gráfico | 
| `ethernets: enp1s0:` | Definimos la interfaz | 
| `dhcp4: no` | imprescindible para una IP estática. |

Luego aplicamos los cambios:

        sudo netplan apply

>Mantener una dirección estática es esencial para que todos los servicios del ecosistema puedan comunicarse de forma predecible. Además, simplifica la administración, las reglas de firewall y la resolución de nombres locales como los dominios .lan en el archivo de `Caddyfile`.

#### 2.Instalar utilidades
Instalamos algunas utilidades base con:

        sudo apt install curl git vim htop net-tools ufw -y
<p align="center">
  <img src="../../assets/img/ubuntu/util.png" alt="ubuntu" width="500">
</p>

### Montaje de disco externo

Debido a que nuestro mini-pc tiene 415gb optamos por añadir un disco duro externo para ampliar el almacenamiento del sistema, identificado como dexterno, utilizado principalmente para el almacenamiento de Nextcloud. Para montar un disco deberemos de seguir los siguientes pasos: 

#### 1. Verificación del dispositivo:

        sudo fdisk -l

<p align="center">
  <img src="../../assets/img/ubuntu/disk1.png" alt="ubuntu" width="500">
</p>

y localizamos el disco que queremos montar.

#### 2. Creación del punto de montaje:

        sudo mount /dev/sda1 /mnt/dexterno

#### 3. Montaje manual y prueba:

        sudo mount /dev/sda1 /mnt/dexterno

#### 4. Montaje automático al iniciar el sistema:
Editamos /etc/fstab y añadimos la siguiente línea:

        /dev/sda1   /mnt/dexterno   ext4   defaults   0   2

<p align="center">
  <img src="../../assets/img/ubuntu/fstab.png" alt="/etc/fstab" width="600">
</p>


#### 5. Comprobación final:

Ejecutamos el siguiente comando para verificar que el disco dexterno esté correctamente montado y accesible.

        df -h

<p align="center">
  <img src="../../assets/img/ubuntu/df-h.png" alt="df -h" width="600">
</p>

Con este comando verificamos que el disco dexterno esté correctamente montado y accesible.


### Estructura de carpetas del proyecto
Para dejar preparadas carpetas que gestionaremos a través de Dockge ejecutaremos:

        sudo mkdir -p /srv/docker/stacks
        sudo chown -R $USER:$USER /srv/docker

<p align="center">
  <img src="../../assets/img/ubuntu/stacks.png" alt="/srv/docker/stacks" width="600">
</p>

> Una vez creada la carpeta donde irán los servicios, [Dockge](./dockge.md) se encargará de la gestión de las carpetas y archivos de cada servicio que necesitemos. 



Y también creamos la carpate para el disco `dexterno` para almacenamiento adicional para nextcloud:

        sudo mkdir -p /mnt/dexterno/nextcloud

<p align="center">
  <img src="../../assets/img/ubuntu/dexterno.png" alt="/srv/docker/stacks" width="500">
</p>


## Verificación final

Ahora para comprobar que todo lo que hemos realizado se ha configurado correctamente haremos las siguientes comprobaciones:

        lsb_release -a

<p align="center">
  <img src="../../assets/img/ubuntu/lsb.png" alt="/srv/docker/stacks" width="500">
</p>
        docker --version

<p align="center">
  <img src="../../assets/img/ubuntu/docker-version.png" alt="/srv/docker/stacks" width="500">
</p>

        sudo systemctl status ssh

<p align="center">
  <img src="../../assets/img/ubuntu/ssh.png" alt="/srv/docker/stacks" width="600">
</p>

        ip -4 addr show enp1s0 | grep inet

<p align="center">
  <img src="../../assets/img/ubuntu/ip-show.png" alt="/srv/docker/stacks" width="500">
</p>

        df -h | grep dexterno

<p align="center">
  <img src="../../assets/img/ubuntu/df-hgrep.png" alt="/srv/docker/stacks" width="600">
</p>

## Conclusión

Ubuntu Server nos proporciona una muy buena base sobre la que montar BitCLD. Es estable, seguro y compatible con una gran cantidad de herramientas lo que lo convierte en un muy buen sistema operativo para nosotros ya que suple todas nuestras necesidades en cuanto a SO.
El configurar una IP fija, en nuestro caso `192.168.1.5` simplifica enormemente la administración y el añadido del disco duro externo nos ofrece una gran capacidad y la posibilidad de despreocuparnos del almacenamiento.