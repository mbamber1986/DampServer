## docker-compose.yml ##
```
version: "3.2"
networks:
  backend:
services:
  php:
    container_name: phpapp
    image: mbamber1986/php
    networks:
      - backend
    restart: always
    volumes:
      - ./path/to:/var/www/html
```