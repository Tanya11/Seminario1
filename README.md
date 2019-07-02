# Servidor web con nginx
Contenido de fase 1 del proyecto de seminario de sistemas vacaciones de junio 2019


Pasos para crear la pagina web
apt-get install docker.io
-ahora crear una carpeta  que sera nuestro proyecto
$ mkdir paginadocker
 Ingresar al archivo y colocar lo siguiente
$nano Dockerfile
Dockerfile

FROM ubuntu:18.04
RUN apt-get update
RUN apt-get install -y nginx
WORKDIR /var/www/html
RUN rm *
COPY index.html .
#ADD carpeta /root/
EXPOSE 80
#ENV VAR_ENTORNO contenido
CMD /usr/sbin/nginx -g "daemon off;"

EN la misma carpeta crar el un index.html 
$nano index.html

Y poner el siguiente html 

HOLA COMO ESTAN ESTA ES UNA PAGINA WEB 

O se puede poner simplemente el html de la pagina que se va a utilizar
Ssiempre dentro de la carpeta creamos la imagen con el siguiente comando

$ docker build -t . tanyarecinos/apache2:latest

ERROR SI APARECE QUE NOS E PUEDE CREAR ENTONCES AGREGAR EL PUNTO DE ULTIMO DE ESTA FORMA

$docker build -t tanyarecinos/apache2:latest .
Ahora para subir la imagen tenemos que loguearnos

docker login

Podemos ver que si se creo la imagen dando el comando
$docker images
Y corremos el conteiner 
$ docker run -p 8002:80 -d -it tanyarecinos/paginasirve
y ya logueados subimos la imagen a docker hub con:

$docker push tanyarecinos/apache2:latest

despues para hacer la prueba de que sirve podemos descargar la immagen con el comando
$docker pull tanyarecinos/apache2:latest
y luego le damos run
a la imagen que descargamos 
$ docker run -p 8002:80 -d -it tanyarecinos/tagname

para ver en que puerto esta corriendo le damos 
$docker inspect %ID
donde el ID son los primeros 4 caracteres del conteiner que sse crea despues de darle run
FIN

