# symfony_docker
Quick Start

Crie uma pasta com o nome de symfony_docker;
Entre na pasta symfony_docker
Utilize o docker-compose

version: '3.8'

services:
  database:
    container_name: database
    image: mysql:8.0
    command: --default-authentication-plugin=mysql_native_password
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: symfony_docker
      MYSQL_USER: symfony
      MYSQL_PASSWORD: symfony
    ports:
      - '3306:3306'
    volumes:
      - ./mysql:/var/lib/mysql
    networks:
      - symfony
  php:
    container_name: php
    build:
      context: ./php
    ports:
      - '9000:9000'
    volumes:
      - ./app:/var/www/symfony_docker
    depends_on:
      - database
    networks:
      - symfony      
  nginx:
    container_name: nginx
    image: nginx:stable-alpine
    ports:
      - '80:80'
    volumes:
      - ./app:/var/www/symfony_docker
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - php
      - database
    networks:
      - symfony

networks:
    symfony:
    
