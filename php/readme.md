# How to use this php7.4 fpm file

## the env file

```
container_name=
workdir= /var/www/html

```

``` Docker Compose file
version: "3.2"
services:
  php:
    image: mbamber1986/php
    networks:
      - backend
    working_dir: ${workdir}
    volumes:
      - ./public_html/:/var/www/html

```

## required flags for this to work

``` Required flags

image: mbamber1986/php-fpm
volumes:
  - /path/to/local/source:/path/to/root/of/sever  // /mnt/c/storage/web/html:/var/wwww/html
```

## optional Flags

```
// this is the same as WORKDIR  if you are linking a .env file you simply add ${workdir} instead of the path name allowing .env to take over

working_dir:/var/www/html // change this start from a different work directory.
container-name:${container_name}

//no always, on-failure, unless-stopped
restart: 

```
c
# installation

```

docker-compose up

// additional flags

-d  for detached mode
--force-recreate  to create from the downloaded image
```

## Small footnote

the .env file is put in place for simplicty rather than a requiment if you choose to not use or include a .env file simply replace the enviroment variables with your own names  enviroment variables look like the following: ${a_env_name}