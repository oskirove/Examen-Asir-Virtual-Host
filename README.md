# Examen Dockerfile y VirtualHost

## Creación de una Imagen Alpine con Navegador de Terminal Integrado

Para iniciar la creación de nuestra **imagen personalizada** de **Alpine**, comenzaremos por crear nuestro archivo **`Dockerfile`**. En este archivo, incluiremos los siguientes parámetros:

```dockerfile
# Utilizamos la imagen más reciente de Alpine como base
FROM alpine:latest

# Especificamos el tipo de terminal que vamos a emular en nuestro contenedor
ENV TERM linux

# configuramos la imagen para que actualice los 
# paquetes e instale el navegador de terminal (links en este caso)
RUN apk update && apk add links

# Establecemos el comando predeterminado que se ejecutará 
# de manera automática al iniciar el contenedor
CMD ["links"]

```
Una vez hayamos concluido la configuración de nuestro archivo, estaremos listos para **crear la imagen** de nuestro contenedor personalizado.

> [!NOTE]
> Para crear nuestra imagen utilizaremos el comando **`docker build -t alpine-navegador .`**

Como último paso **crearemos** el contendor utilizando el siguiente comando:

> docker run -it alpine-navegador

Después, verificaremos que el contenedor funciona correctamente y, posteriormente, procederemos a subir nuestra imagen a nuestro repositorio de **Docker Hub**.

![Imagen que comprueba el funcionamiento del contenedor Alpine](alpineNavegador)

> [!NOTE]
> [Enlace al repositoio de Dockerhub](https://hub.docker.com/r/oskiroveasir/examen-asir)

## Configuración de dos Virtualhost Diferenciados por el Puerto

Agregamos al httpd.conf lo siguiente:

```conf
# Configuración del primer virtual host
<VirtualHost *:80>
    ServerName localhost
    DocumentRoot /var/www/html/vh1
    
    # Definimos la ruta donde va a tener que 
    # buscar la página que queremos mostrar
    <Directory "/var/www/html/vh1">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>

Listen 8000
# Configuración del segundo virtual host
<VirtualHost *:8000>
    ServerName vhost2
    DocumentRoot /var/www/html/vh2
    # Definimos la ruta donde va a tener que 
    # buscar la página que queremos mostrar
    <Directory "/var/www/html/vh2">
        AllowOverride All
        Require all granted
    </Directory>
</VirtualHost>
```

Una vez configuremos los virtualhost definiremos lo siguiente:

```conf
<IfModule dir_module>
    DirectoryIndex vh1.html vh2.html
</IfModule>
```

![imagen de los virtualhost funcionando en diferentes puertos](virtualhost-nav)