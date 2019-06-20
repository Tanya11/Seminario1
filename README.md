# Servidor web con nginx
Contenido de fase 1 del proyecto de seminario de sistemas vacaciones de junio 2019


CREAR UN CONTENEDOR PARA NGINX

Se descarga la imagen de nginx con el comando
1. docker pull nginx 
2. docker run -d \ -p 0.0.0.0:12880:80 \ -v <carpeta de contenido>:<cualquier carpeta docker> \ nginx:1.12.1

aprovechamos también para cambiar la carpeta por defecto de Nginx a /code.
3.Creamos un archivo Dockerfile en nuestra carpeta de proyecto con el siguiente contenido:

FROM nginx:1.14.0
MAINTAINER Albert Lacarta <albert.lacarta@rqlogic.com>

ADD nginx_default /etc/nginx/conf.d/default.conf
RUN mkdir -p /code


4.Añadimos en la misma carpeta que el Dockerfile, el archivo nginx_default con la siguiente configuración:

server {
    # Escuchamos todas las conexiones en el puerto 80    
    listen 80 default;

    # Carpeta donde reside el contenido estático
    root /code;

    index index.html;

    # Definimos la acción a tomar cuando sea un error 404, referido al location @custom404
    error_page 404 @custom404;

    # Compresión GZIP para reducir el uso de ancho de banda
    gzip             on;
    gzip_comp_level  2;
    gzip_min_length  1000;
    gzip_proxied     expired no-cache no-store private auth;
    gzip_types       text/plain application/x-javascript text/xml text/css application/xml;

    # Redirección con código 301 (redirección permanente)
    location @custom404 {
        #Reemplazar con el dominio del host
        rewrite .* http://tudominio.com permanent;
    }

    # Intenta cargar la $uri recibida, y si no la encuentra, carga la redirección a / con 301
    location / {
        try_files $uri $uri/ @custom404;
    }

    # Ficheros de log
    error_log /var/log/nginx/project_error.log;
    access_log /var/log/nginx/project_access.log;
}

5. Podemos construir la imagen para guardarla en nuestro repositorio local de Docker con :

docker build -t rqlogic/superimagen-nginx .
6. volvemos a ejecutar el run, pero ahora con el nuevo directorio y el nombre de la imagen:

docker run -d \
    -p 0.0.0.0:12880:80 \
    -v /home/user/workspace/superproject/html:/code \
    rqlogic/superimagen-nginx
