<!-- Write about the project  -->
<!-- How to use the docker-compose file -->
<!-- How to install laravel -->
<!-- how to use docker exec -->
<!-- Optional flags -->
<!-- Envirmental variables -->

# About My Docker Based Lamp Server
this project was built by myself simply because i didnt like the restrictions or limitations when i was using laragon or homestead to create my own laravel and php based projects, from this point i was then instroduced to docker and from there i felt this was the way to go.

I have built this project for personal use but feel if it can help others they are free to use or customise it hence why i am sharing all the associated files with it.

## Whats included

Apache (httpd 2.4.46)
php 7.4-fpm
mysql (default image from docker)
composer support via php fpm
laravel support


## Future plans 
i am currently looking at adding node js package manager via nvm

## The .Env files and how they are related

```
container_prefix=container_prefix
web_root=/var/www/html/test
workdir=/var/www/html

```

## The Docker-compose base file and what is needed

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
      - ./public_html/:${workdir}
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
<!--  Add information about this section-->
simply run the command  from the root folder where your docker-compose.yml file is held: 
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


#Support with ngrok

```

  ngrok:
    image: shkoliar/ngrok:latest
    container_name: ${container_prefix}_ngrok
    links: 
      - apache
    ports:
      - 4551
    networks:
      - web
      - backend
    environment:
      - DOMAIN=apache
      - PORT=80
    restart: always

```
