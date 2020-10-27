```Laravel Installer
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
  web
  backend:
services:
  php:
    container_name: ${container_php
    image: mbamber1986/php
    networks:
      - backend
    restart: always
    working_dir: ${workdir}
    volumes:
      - ./yourcoderoot /: ${workdir}
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
