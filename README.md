# dockerCompose
## Subida del proyecto a Docker

En este repositorio mostraremos la subida del proyecto a Docker. Constará de dos imágenes:
Tomcat
MySQL

Para ello, usaremos un archivo llamado docker-compose.yml. Este archivo será el que 'pullearemos' desde el github donde tenemos el proyecto. A través de él, crearemos las imágenes.

### Git Clone :

<img width="296" alt="clone_git" src="https://user-images.githubusercontent.com/91744554/172452841-5961a98c-f9d9-454c-8a64-c118c30b34c8.png">

El archivo .yml que estamos clonando es el siguiente: 
```
version: '3.3'
services:
  db:
    image: mysql:8.0
    volumes:
      - ./test:/var/lib/mysql
      - ./bbdd:/docker-entrypoint-initdb.d
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: Casino
      MYSQL_USER: prueba
      MYSQL_PASSWORD: 123456
    ports:
      - 3307:3306
    cap_add:
      - SYS_NICE
  phpmyadmin:
    depends_on:
      - db
    image: phpmyadmin/phpmyadmin
    ports:
      - '8081:80'
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: root
  web:
    depends_on:
      - db
    image: tomcat:9.0
    volumes:
      - ./ProyectoCasino.war:/usr/local/tomcat/webapps/PokemonFBM.war
    ports:
      - '8080:8080'
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: Casino
      MYSQL_USER: prueba
      MYSQL_PASSWORD: 123456 ```
      
Sin conocimiento del porqué, nos salta el siguiente error al no encontrar el fichero: 

<img width="679" alt="error" src="https://user-images.githubusercontent.com/91744554/172452898-8614e0d9-d696-4230-99a8-5004d2949c31.png">


