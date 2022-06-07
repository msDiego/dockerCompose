# dockerCompose
## Subida del proyecto a Docker

### Introducción

En este repositorio mostraremos la subida del proyecto a Docker. Constará de dos imágenes:
Tomcat
MySQL
nGinx

### Requisitos previos antes de realizar la subida

Para poder realizar esta subida, debemos tener instalado en nuestra máquina docker. Lo podemos installar a través del comando ``apt install docker``.
Además de docker instalado en nuestra máquina, debemos tener un código java (que conformará el backend) con la extensión .war, que será lo que despleguemos.
También, los ficheros HTML que constituyen nuestro frontend para poder desplegarlos a través de nGinx

### Guía para subida de proyecto

Usaremos un archivo llamado docker-compose.yml. Este archivo será el que 'pullearemos' desde el github donde tenemos el proyecto. A través de él, crearemos las imágenes. Este archivo se basa en lo siguiente:
- version: aquí pondremos la versión de docker con que trabajamos.
- services: a continuación de esta etiqueta definiremos las imágenes a utilizar.
- db: hace referencia a la imágen que usaremos como base de datos.
- image: nombre de la imágen y versión de forma opcional, por defecto tomará el valor de :latest que se referirá a la última versión.
  volumes: Aquí definiremos los archivos a desplegar en el contenedor, en caso de una imágen de base de datos pondremos la ruta en que queremos que almacene los
  datos.
- environment: Dentro de esta etiqueta definiremos las variables del servidor y sus valor como pueden ser los usuarios y contraseñas.
ports: A continuación de esto definiremos el puerto en que se desplegará en contenedor, si ponemos dos puntos seguidos de otro puerto este hará referencia al mismo puerto de nuestra máquina lo que nos permitirá interactuar con el contenedor desde fuera de docker.

### Git Clone :

<img width="296" alt="clone_git" src="https://user-images.githubusercontent.com/91744554/172452841-5961a98c-f9d9-454c-8a64-c118c30b34c8.png">

El archivo .yml que estamos clonando es el siguiente: 
```
version: '3.3'
services:
  db:
    image: mysql:8.0
    volumes:
      - /var/lib/mysql
      - ./bbdd:/docker-entrypoint-initdb.d
    environment:
      MYSQL_ROOT_PASSWORD: cashConcos
      MYSQL_DATABASE: Casino
      MYSQL_USER: admin
      MYSQL_PASSWORD: cashConcos
    ports:
      - 3306:3306
  backend:
    depends_on:
      - db
    image: tomcat:9.0
    volumes:
      - ./ProyectoCasino.war:/usr/local/tomcat/webapps/ProyectoCasino.war
    ports:
      - '8080:8080'
web:
  image: nginx 
  ports: 
  - "80:80"
  volumes:
  ./front:/usr/share/nginx/html
 ```
Una vez tenemos este archivo, arrancamos los contenedores con el comando ``docker-compose up -d``
Se rastreará el archivo .yml y se usará de referencia.
Nos salta un error que no sabemos solucionar, así que explicaremos el procedimiento sin imágenes debido al error:

<img width="679" alt="error" src="https://user-images.githubusercontent.com/91744554/172457275-bb0398bb-e699-44cb-83a3-09fbdb368df3.png">

### Subir las imágenes a Docker Hub
El siguiente paso a realizar sería subir nuestras imágenes. Para ello, debemos tener un usuario registrado en Docker Hub, al cual accederemos con el siguiente comando: 
``docker login``

Una vez realizado el login, seleccionamos las imágenes locales que deseamos subir. En este caso, las listamos con ``docker images``
A estas imágenes, les asignaremos un tag con ``docker tag`` . 
Por ejemplo, podemos usar ``docker tag tomcat:9.0 diegoexamen/sic.tomcat`` .
Una vez establecidas las tags, hacemos un push de la siguiente manera: 
``docker push diegoexamen/sic.tomcat``

### Visualización de las imágenes en docker hub

El proceso anterior iniciará un proceso de subida que desemboca en la posibilidad de visualización de la imagen correspondiente en nuestro repositorio de Docker Hub.
Desgraciadamente, por la imposibilidad de completar el ejercicio debido al error anteriormente encontrado, no podemos presentar una imagen de como quedaría, aunque aquí tienes un ejemplo: 

<img width="860" alt="dockerhub" src="https://user-images.githubusercontent.com/91744554/172458197-67108e3e-9e19-4aba-81f5-dd145370a28c.png">

### Ejemplo del proyecto completamente desplegado

La visualización de nuestro proyecto, quedaría de la siguiente manera: 

<img width="960" alt="previsualización" src="https://user-images.githubusercontent.com/91744554/172459499-467874d7-63e6-4488-8321-2dadc0c5353c.png">

Aunque, otra vez, debido al error que hemos tenido en esta sesión, no tenemos una ip que otorgar para poder ver el proyecto finalizado. Aún así, accediendo a través de 162.19.66.62 , se podrá ver el proyecto completamente desplegado de la misma forma que hemos visto en este examen. 

### Conclusiones

Podemos considerar docker como una herramienta ampliamente útil en el mundo del despliegue de aplicaciones web. Su uso se encuentra a la orden del día debido a su ligero peso, fácil comportamiento y gran utilidad en entornos de trabajo. Aún así, en nuestra experiencia personal, resulta muy frustrante que sea una tecnología tan cerrada y delicada, donde todo debe ir a la perfección. Nos hemos encontrado con errores incomprensibles donde se realizan los mismos pasos que muchos compañeros (Véase el error citado anteriormente, el cual no nos ha permitido continuar con el despligue). Igualmente, a pesar de las frustraciones, consideramos que Docker es una herramienta que vale la pena aprender debido a su utilidad en el mundo del despliegue actual. 




