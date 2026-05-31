# Riesgos y Buenas Prácticas en un Entorno Autoalojado

En este apartado hablaremos sobre los distintos riesgos que consideramos que podemos tener al estar en un entorno mayormente autoalojado y también de buenas prácticas que podemos hacer para paliar al máximo posible, los riesgos. Siendo conscientes de que no existe un sistema 100% seguro y que a pesar de usar tecnologías como Cloudflare siempre pueden haber brechas.

## Riesgos de un entorno autoalojado
En cuanto a los riesgos a los que nos enfrentamos teniendo un entorno autoalojado expuesto a internet consideramos que se pueden dividir en tres principales: seguridad, operativos y de privacidad.

### Riesgos de Seguridad
#### Accesos no autorizados:
Aunque estemos usando Cloudflare tunnel para exponer nuestros servicios, podemos ser víctimas de ciberdelincuentes que intenten acceder de manera ilegítima a nuestro servidor, si no gestionamos bien las políticas en Access de nuestras aplicaciones. 
#### Configuraciones inseguras:
Un error en la configuración de cualquier servicio puede dejarnos expuestos a un ataque sin que nosotros seamos conscientes. 
#### Falta de actualizaciones:
Otro gran error que podemos cometer es no tener actualizado en todo momento nuestro servidor y nuestras aplicaciones.
#### Uso de APIs y accesos:
Además, también debemos tener cuidado con el uso que le damos a nuestras APIs y otros accesos como la cuenta de Cloudflare o GitHub.

### Operativos
#### Pérdida de datos:
Dependemos de nuestro disco duro y ya sea por fallos nuestros o por fallos del servidor, al no disponer de un sistema de RAID o de sincronización automática, podemos perder información y configuraciones importantes.
#### Caída de servicios:
Como solo disponemos un servidor para BitCLD, si el servidor fallara, se reiniciara o le ocurriera cualquier otro problema ya no podríamos acceder a ninguno de nuestros servicios.
#### Sobrecarga del sistema:
Al ser un equipo muy limitado debemos de tener cuidado con no sobrecargar nuestra CPU o RAM y que por ello podamos tener algún error.
#### Dependencia de otras empresas para acceder en remoto:
Cloudflare es muy seguro y nos ofrece un plan gratuito muy generoso, pero dependemos por completo de su red o de la de Tailscale (en casos excepcionales) para poder hacer accesible nuestros servicios al exterior.

### De privacidad
#### Exponer información sensible sin querer:
Debemos de tener cuidado con lo que subimos a GitHub, en en nuestra documentación o en nuestra documentación web ya que podemos estar dando información de más o sensible
#### Sin control de terceros:
No podemos olvidarnos de que empresas como Cloudflare, Tailscale o Groq pueden sufrir ataques y afectarnos indirectamente.

## Buenas Prácticas en un entorno autoalojado

Para intentar paliar todos los riesgos que hemos visto que podemos tener (que no son pocos), debemos poner de nuestra parte y hacer todo lo posible para minimizarlos respecto a los tres principales riesgos anteriores.

### Seguridad
#### Usar siempre Cloudflare Access:
Debemos usar, en todas nuestras aplicaciones y en todo momento, Cloudflare Access con una política lo más restrictiva posible. En nuestro caso mediante sólo tres emails posibles y con restricción de país. 
#### Autenticación 2FA:
Para proteger nuestras cuentas es casi imprescindible que establezcamos el doble factor de autenticación en nuestras cuentas de Cloudflare, GitHub, Tailscale y Groq.
#### Cambio periódico de APIs y tokens:
Por seguridad también es recomendable que de vez en cuando cambiemos nuestras credenciales como con Tailscale, Groq, o incluso de manera esporádica nuestro propio túnel de cloudflared.
#### Usar HTTPS: 
Siempre que sea posible usar nuestros servicios mediante su subdominio de bitcld.com o mediante los dominios .lan para que así nuestro tráfico siempre esté cifrado gracias a Cloudflare Tunnel o a Caddy.
#### Mantener el servidor y servicios actualizados:
Debemos asegurarnos de que en todo momento tenemos nuestro sistema operativo y nuestros servicios actualizados para evitar vulnerabilidades.

### Operativas
#### Hacer copias de seguridad periódicamente:
Para evitar al máximo posible la pérdidas de datos debemos de hacer copias de seguridad de nuestros datos, idealmente que no estén en nuestro servidor como por ejemplo en nuestro ordenador o en otra nube como Mega o Proton drive. Además de guardar todas nuestras configuraciones más importantes de cada servicio.
#### Monitorización del servidor:
Para evitar que el equipo se sobrecargue es importante que monitoreemos nuestro servidor para asegurarnos que no estamos haciendo un uso excesivo de la CPU, RAM u otro componente. Para ello tenemos a Netdata y el envío de notificaciones en nuestro grupo de Telegram.
#### Documentar todos los cambios:
Algo interesante también es tener un documento, una vez entreguemos la documentación, en el que añadamos todos los cambios que hagamos posteriormente a la entrega. Para así poder tener un registro de bitCLD.

### De Privacidad
#### Exponer información sensible:
Debemos siempre evitar almacenar configuraciones sensibles en GitHub, sobretodo en repositorios públicos, o incluso en nubes externas. En caso de ser necesario deberemos cifrarlas antes de subirla.
