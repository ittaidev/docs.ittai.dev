# Cloudflare Pages


Ya hemos hablado de todas las ventajas que nos ofrece la Plataforma de Cloudflare. Es por 
ello que sólo añadiremos el proceso de publicación de una página web mediante Cloudflare 
Pages, en nuestro caso será la documentación del proyecto. De esta manera, así podemos 
trabajar con GitHub y Cloudflare Pages y así decidir el que mejor nos convenga.

## Creación del repositorio en GitHub 

Para ello primero deberemos crear un repositorio en GitHub, no importa si es público o 
privado ya que le daremos acceso a Cloudflare Pages directamente al repositorio. 

<p align="center">
  <img src="../../assets/img/pages/paso1.png" alt="Cloudflare Pages" width="600">
</p>

Una vez creado subiremos nuestros archivos, para Cloudflare instale MkDocs 
necesitaremos añadir un documento en la raíz del repositorio que se llame requeriments.txt 
con el siguiente contenido: 

        Mkdocs 
        mkdocs-material

<p align="center">
  <img src="../../assets/img/pages/paso2.png" alt="Cloudflare Pages" width="700">
</p>


## Despliegue de la web 

Una vez hayamos creado nuestro repositorio, en Cloudflare, abriremos Workers & Pages > 
Create Application. Y seleccionamos la opción de Pages que encontramos abajo: 

<p align="center">
  <img src="../../assets/img/pages/paso4.png" alt="Cloudflare Pages" width="600">
</p>

1. Una vez en Pages, conectaremos nuestra cuenta de GitHub con Cloudflare y 
seleccionamos el repositorio que queremos publicar. En nuestro caso le dimos acceso 
restringido a los repositorios por eso sólo se ven el de la documentación, nuestra web 
principal y el de pruebas. <p align="center"><img src="../../assets/img/pages/paso3.png" alt="Cloudflare Pages" width="600"></p>  <p align="center"><img src="../../assets/img/pages/paso5.png" alt="Cloudflare Pages" width="600"></p>



2. Elegimos un nombre para el proyecto (en nuestro caso el mismo que el del repositorio) y 
en la configuración añadimos: 

        ➔ Production branch: main (ya que es la única rama tenemos) 

        ➔ Framework preset: none 

        ➔ Build command: pip install -r requirements.txt && mkdocs build 

        ➔ Build output directory: site 

    <p align="center"><img src="../../assets/img/pages/paso6.png" alt="Cloudflare Pages" width="600"></p>

    Y en environment variables (advanced) añadimos: 

        ➔ PYTHON_VERSION = 3.11 

    <p align="center"><img src="../../assets/img/pages/paso7.png" alt="Cloudflare Pages" width="600"></p> 

3. Una vez añadida las configuraciones Cloudflare Pages comenzará a construir y desplegar 
nuestra web: 

    <p align="center"><img src="../../assets/img/pages/paso8.png" alt="Cloudflare Pages" width="600"></p> 

4. Cuando finalice ya podremos acceder a nuestra web desde el enlace que nos 
proporciona la plataforma [bitcld-docs.pages.dev](https://bitcld-docs.pages.dev/):

    <p align="center"><img src="../../assets/img/pages/paso9.png" alt="Cloudflare Pages" width="600"></p> 

5. Sin embargo, lo que nosotros buscamos es usar nuestro dominio del proyecto para que 
quede más profesional y coherente. Elegimos docs.bitcld.com . Para ello seleccionamos la 
opción de Add custom domain: 

    <p align="center"><img src="../../assets/img/pages/paso10.png" alt="Cloudflare Pages" width="600"></p> 

6. Añadimos el subdominio que queremos: 

    <p align="center"><img src="../../assets/img/pages/paso11.png" alt="Cloudflare Pages" width="600"></p> 

7. Al tener nuestro dominio en Cloudflare, la plataforma se encargará de generar el registro 
DNS automáticamente: 

    <p align="center"><img src="../../assets/img/pages/paso12.png" alt="Cloudflare Pages" width="600"></p> 

8. Después ya sólo nos queda esperar a que replique. En nuestro caso fue bastante rápido. 

    <p align="center"><img src="../../assets/img/pages/paso13.png" alt="Cloudflare Pages" width="600"></p> 
    <p align="center"><img src="../../assets/img/pages/paso14.png" alt="Cloudflare Pages" width="700"></p> 

Una vez haya replicado ya podremos acceder a nuestra documentación web hecha en 
Mkdocs con el tema de Material desde nuestro subdominio:

<p align="center"><img src="../../assets/img/pages/paso15.png" alt="Cloudflare Pages" width="700"></p> 
