## Using our docker file to run a Docker based lamp server with Laravel support

Our  Lamp server is based on docker debian based linux httpd 2.4.46 and php 7.4-fpm using docker images mbamber1986/apache and mbamber1986/php if you wish to create your own versions of php-fpm and apache for your personal customisation simply download my source code.


```Docker Lamp server
// using my .env file to link both containers together

//if you wish to use a universal container name ie yourwebsite_php yourwebsite_apache user the ${container_prefix} variable inside your docker-compose file
container_prefix=your_prefix_name

//the website root where your website will be held this will allow apache to read the website and allow php-fpm to communicate with apache
www_root=/var/www/html/public_html 

// the working dir which you will start from when using docker exec in my setup i have used /var/ww/html as my route directory
workdir = /var/www/html


// this installer will requires both the apache and php-fpm repositories 
version: "3.2"
networks:
  web:
    external: true
  backend:
services:
  php:
    container_name: ${container_prefix}_php
    image: mbamber1986/php
    networks:
      - backend
    restart: always
    working_dir: ${workdir}
    volumes:
    #set your path to your own code in our example we used ./public_html  which is  in our source code change this to what ever your need to making sure your have the correct permissions
      - ./public_htmk:${workdir} 
  apache:
    container_name: ${container_prefix}_apache
    image: mbamber1986/apache
    ports:
      - 80:80
      - 443:443
    working_dir: ${workdir}
    environment:
      - workdir=${workdir}
      - web_root=${web_root}
    depends_on:
      - php
      - mysql
    networks:
      - web
      - backend
    restart: always
    volumes:
      - ./public_html/:${workdir}
  mysql:
    container_name: ${container_prefix}_mysql
    image: mysql:5.6.40
    volumes:
      - ./mysql/data:/var/lib/mysql
    networks:
      - backend
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=rootpassword
```

# Instalation

simply run the command  from the root foler where your docker-compose.yml file is held: 
```
docker-compose up -d
``` 
if you wish to recreated any created instances use --force-recreate

## installing Laravel

Our package has support for laravel and will work by doing the following seteps

### Step 1 : enter docker exec

use the following commands to enter docker exec

```
docker exec -it container_name /bin/bash

```
### step 2: installing laravel

from the root of your working directory in our instance /var/www/html enter 
```
composer create-project --prefer-dist laravel/laravel src
```
