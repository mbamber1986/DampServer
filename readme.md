# (DAMPSERVER)  A linux apache php-fpm server built for Docker #

# Introductiuon
The DAMPserver is  a work in progress project that is designed to simply deploy websites with ease using .env and variables which are linked with apache vhost files as well as the docker-compose file  allowing for the easy deployment of websites in a quick and easy manner.

I have chose to release this package as an open source project  allowing others to customise this project to your own needs and delploy there own websites with ease.

# Current Support 
```
php 7.4 fpm
apache 2.4.6
mysql 
composer and laravel support( this has to be run via the php container)

```
# The base env files
## Adding your own varibales to the base file is encouraged to give you a personal experience
```
<!-- #container name prefix : as i usually use the names that the containers use ie apache php ngrok etc i chose to use this variable and add a prefix, in my setup i also used this to link my folders on my local file server. -->

container_prefix=examplewebsite

<!-- this variable is simply pushed to the apache container telling the sever where to read the apache files from, the apache server will then comunicate with php-fpm -->

web_root=/var/www/html/test

<!-- under my setup i used this variable to link my start point of the php and apache containers and also the way my volumes linked the files between the containers remote files and the local files. -->

workdir=/var/www/html
```
# The Base Docker file

## here is my base file i have chosen to leave the variables in to show how they link with the env files please feel free to amend them if you see fit.
```
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
      - ./www/:${workdir}
  apache:
    container_name: ${container_prefix}_apache
    image: mbamber1986/apache
    working_dir: ${workdir}
    environment:
      - workdir=${workdir}
      - web_root=${web_root}
    ports:
      - 80:80
      - 443:443
    depends_on:
      - php
      - mysql
    networks:
      - web
      - backend
    restart: always
    volumes:
      - ./www/:${workdir}
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

  # installation of the docker file 

  ```
  simply use the following commands 

  docker-compose up -d

  additional flags that can be used are 

  --forece-recreate
  --remove-orphans
```

# Installing laravel


## from the root of your working directory in our instance /var/www/html enter 

```
composer create-project --prefer-dist laravel/laravel src

```

# entering the container 

i have used the following the comand to enter into the docker container  using interactive mode use the container name you have used 

to get this name simply use docker ps

```
docker exec -it container_name /bin/bash
```


# optional setups that i added to my server 

### traefik
using this will require the installation of traefik and will require the removal of ports from the docker compose files
Although not listed in the base env files i used a variable called domain the entry points are located in the traefik installation file refer to traefik docmentation for more information : https://doc.traefik.io/traefik/getting-started/install-traefik/#use-the-official-docker-image

```
labels:
  - "traefik.enable=true"
  - "traefik.http.routers.${container_prefix}_web.rule=Host(`${domain}`)"
  - "traefik.http.routers.${container_prefix}_web.entrypoints=web"
  - "traefik.http.routers.${container_prefix}_websecure.rule=Host(`${domain}`)"
  - "traefik.http.routers.${container_prefix}_websecure.entrypoints=websecure"
  - "traefik.http.routers.${container_prefix}_websecure.tls.certresolver=myresolver"
  - "traefik.docker.network=web"

```

### NGrok

```

ngrok:
  image: shkoliar/ngrok:latest
  container_name: ${container_prefix}_ngrok
  links: 
    - apache
  ports:
    - 4551:4551
  networks:
    - web
    - backend
  environment:
    - DOMAIN=apache
    - PORT=80

```



