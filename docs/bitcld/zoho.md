# Zoho Mail

## Introducción  
Zoho Mail es una plataforma de correo electrónico profesional que forma parte del ecosistema de aplicaciones en la nube de Zoho. A diferencia de otros servicios de correo como Google, Outlook… Zoho nos ofrece un correo electrónico sin anuncios, con una interfaz moderna y una buena integración con dominios personalizados. 

## Motivos de elección  
A pesar de no ser un servicio autoalojado, y por tanto incumplir uno de los pilares fundamentales de nuestro proyecto. Escogimos Zoho debido a que no disponíamos de mucho más tiempo para preparar un servidor de correo autoalojado pero aún así, al disponer de un dominio personalizado, queríamos aprovechar la oportunidad y crear un correo profesional ya que nunca habíamos tenido la oportunidad de hacerlo. 

Además, de entre las opciones que existen escogimos Zoho mail debido principalmente a la oportunidad de crear hasta 5 cuentas con un dominio personalizado sin coste alguno y a su interfaz limpia, moderna y sin anuncios. Algo que de lo que también salimos beneficiados es que de paso aprovechamos la alta disponibilidad, seguridad y reputación que nos ofrece y que no tendríamos si lo autoalojaramos en un minipc.


## Función dentro del proyecto  
Dentro de bitCLD, Zoho cumple dos misiones fundamentales. La primera y más evidente es la de correo de contacto principal del proyecto mediante `contacto@bitcld.com `para cualquier comunicación formal si fuera necesaria y sobretodo para dar una imagen mucho más profesional al proyecto. Y también para probar como funciona el funcionamiento de un correo profesional aprendiendo cómo funciona.

## Instalación y configuración  

A continuación se resumen los pasos seguidos para la configuración del dominio `bitcld.com` con Zoho Mail:

### 1. Registro en Zoho Mail  
- Acceder a [https://www.zoho.com/es-xl/mail/](https://www.zoho.com/es-xl/mail/)
- Seleccionar la opción de Business Email  
- Seleccionar el plan "Mail Lite – Free for custom domains" (gratuito hasta 5 usuarios)  
- Crear una cuenta de administrador

### 2. Verificación del dominio  
Zoho necesita que demostremos que somos propietarios del dominio. Esto lo hacemos añadiendo un registro TXT en los registros de DNS de nuestro proveedor, en nuestro caso Cloudflare. Con el código que nos indique Zoho y una vez añadido, después de replicar que tarda un poco, ya podremos verificarlo desde el panel de Zoho.

        Tipo: TXT

        Nombre: <el que ofrece zoho>

        Valor: <código_de_verificación>

<p align="center">
  <img src="../../assets/img/zoho/ver.png" alt="Registros DNS completos" width="600">
</p>

### 3. Configuración de registros MX  
Para recibir correos correctamente, es necesario añadir los registros **MX** que Zoho indica:

| Tipo | Nombre | Prioridad |
|------|---------|------------|
| MX | @ | 10 | mx.zoho.eu |
| MX | @ | 20 | mx2.zoho.eu |
| MX | @ | 50 | mx3.zoho.eu |


### 4. Autenticación (SPF, DKIM)  
Para mejorar la entrega y seguridad del correo, se configuraron los siguientes registros:

#### SPF
v=spf1 include:zoho.eu ~all

#### DKIM
Zoho genera un registro tipo TXT con nombre `zoho._domainkey` y un valor único, que debe añadirse al DNS.


Una vez hayamos finalizado, los registros deben aparecer así:

<p align="center">
  <img src="../../assets/img/zoho/registros-dns.png" alt="Registros DNS completos" width="900">
</p>

<p align="center"><em>Registros DNS completos</em></p>


>En caso de usar Cloudflare como gestor de dominio. Para que funcione correctamente debemos desactivar el proxy nube naranja en estos registros ya que los servicios de correo requieren resolución directa hacia los servidores de Zoho y no a través de Cloudflare.

<p align="center">
  <img src="../../assets/img/zoho/nube-proxy.png" alt="No usar cloudflare como proxy" width="300">
</p>
  

Una vez completada la configuración de los registros DNS y la verificación del dominio, creamos una cuenta de contacto y ya tendremos una cuenta de correo funcional. 

<p align="center">
  <img src="../../assets/img/zoho/cuenta-zoho.png" alt="Cuenta Zoho" width="400">
</p>



## Conclusión  
Aunque no es un servicio autoalojado, Zoho Mail nos ofrece estabilidad, alta disponibilidad y una mejor imagen de marca para bitCLD y todo esto sin ningún coste adicional. Además conseguimos también una suite completa y funcional para el trabajo colaborativo, complementaria a Nextcloud. Sabemos que no es lo ideal pero como un extra para poder trabajar con las tecnologías de correo nos pareció una buena adquisición al ecosistema.